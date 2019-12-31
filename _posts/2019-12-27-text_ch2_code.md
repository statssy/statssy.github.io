---
layout: post
title:  "[파이썬] 파이썬으로 배우는 응용텍스트 분석(Ch2. 사용자 정의 말뭉치 구축) 코딩부분"
subtitle:   "PYTHON"
categories: pro
tags: python
comments: true
---

파이썬으로 배우는 응용텍스트 분석 (Ch2. 사용자 정의 말뭉치 구축) 코딩부분 (P21 ~ P40까지 ) 

<img src="http://image.yes24.com/momo/TopCate2739/MidCate008/273872383.jpg" width="30%">  

---

새로운 데이터셋마다 새로운 말뭉치 리더를 작성해야한다.  
다음은 책에서 나온 코드를 해석이다.  
  
---

```python

import os
import nltk
import codecs
import sqlite3

from nltk.corpus.reader.api import CorpusReader
from nltk.corpus.reader.api import CategorizedCorpusReader

## HTML 말뭉치 읽기

"""
범주(categories)와 식별부호(fileids)를 결정하기 위해 정규식을 지정해야한다.
p33에 예시가 있으니 잘 찾아보면 되겠다. 예를 들어 \s는 공백을 말함
"""
# 범주 패턴 정규 표현식
CAT_PATTERN = r'([a-z_\s]+)/.*'
# 문서 패턴 정규 표현식
DOC_PATTERN = r'(?!\.)[a-z_\s]+/[a-f0-9]+\.json'
TAGS = ['h1', 'h2', 'h3', 'h4', 'h5', 'h6', 'p', 'li']

class HTMLCorpusReader(CategorizedCorpusReader, CorpusReader): # 2개를 상속하기 때문에 복잡해질 수 있으므로 init함수들 속의 코드 중 대부분은 단순히 어떤 클래스에 전달할 인수들이 무엇을 드러내는지 역할만 한다.
    """
    원시 HTML 문서들을 전처리하기 위한 말뭉치 리더
    (A corpus reader for raw HTML documents to enable preprocessing.)
    """

    def __init__(self, root, fileids=DOC_PATTERN, encoding='utf8',
                 tags=TAGS, **kwargs): # __init__이란 쉽게 말하면 class 실행시키자마자 돌아가게해줌, kwargs는 keyword argument로 (키워드='특정값')으로 나타냄
        """
        말뭉치 리더를 초기화 한다. 범주 인수들인
        ('cat_pattern', 'cat_map' 및 'cat_file')은
        'CategorizedCorpusReader'라는 생성자(constructor)로 전달된다.
        남은 인수들은 'CorpusReader'라는 생성자로 전달된다.
        """

        # 기본 카테고리 패턴이 클래스로 전달되지 않는다면 기본 카테고리 패턴을 추가한다.
        # (Add the default category pattern if not passed into the class.)
        if not any(key.startswith('cat_') for key in kwargs.keys()): # 여기서 startswith는 key값이 있는지 없는지 체크
            kwargs['cat_pattern'] = CAT_PATTERN

        # NLTK 말뭉치 리더 객체들을 초기화 한다.
        # (Initialize the NLTK corpus reader objects)
        CategorizedCorpusReader.__init__(self, kwargs)
        CorpusReader.__init__(self, root, fileids, encoding)

        # 특별히 추출하기를 바라는 태그들을 저장한다.
        # (Save the tags that we specifically want to extract.)
        self.tags = tags


    def resolve(self, fileids, categories):
        """
        범주가 분류가 되었는지 상관없이
        각 내부 말뭉치 리더 기능으로 전달되는 항목에 따라
        fileid 또는 범주의 목록을 반환한다.
        NLTK의 'CategorizedPlaintextCorpusReader'와 유사하게 구현된다.
        """
        if fileids is not None and categories is not None:
            raise ValueError("Specify fileids or categories, not both")

        if categories is not None:
            return self.fileids(categories)
        return fileids

    def docs(self, fileids=None, categories=None):
        """
        HTML 문서의 전체 텍스트를 반환하고, 문서를 읽고 난 후에는 문서를 닫고
        메모리에 안전한 방식으로 저장한다.
        Returns the complete text of an HTML document, closing the document
        after we are done reading it and yielding it in a memory safe fashion.
        """
        # fileid들과 범주들을 분해한다.
        # Resolve the fileids and the categories
        fileids = self.resolve(fileids, categories)

        # 생성기를 만들어 한 번에 1개 문서를 메모리에 적재한다.
        # Create a generator, loading one document into memory at a time.
        for path, encoding in self.abspaths(fileids, include_encoding=True):
            with codecs.open(path, 'r', encoding=encoding) as f:
                yield f.read()

    def sizes(self, fileids=None, categories=None):
        """
        파일의 튜플, 파일 식별자 및 디스크의 크기 목록을 반환한다.
        이 함수는 말뭉치 중에 유별나게 큰 파일을 탐지하는 데 사용된다.
        Returns a list of tuples, the fileid and size on disk of the file.
        This function is used to detect oddly large files in the corpus.
        """
        # 필드와 범주를 분해한다.
        # Resolve the fileids and the categories
        fileids = self.resolve(fileids, categories)

        # 생성기를 만들고, 모든 경로를 얻고 파일 크기를 계산한다.
        # Create a generator, getting every path and computing filesize
        for path in self.abspaths(fileids):
            yield os.path.getsize(path)

class SqliteCorpusReader(object):
"""
NLTK CorpusReader의 동작을 모방하지만 상속은 하지않음
"""
    def __init__(self, path):
        self._cur = sqlite3.connect(path).cursor()

    def scores(self):
        """
        나중에 지도학습 문제의 표적으로 사용할 감상평 점수를 반환한다.
        Returns the review score
        """
        self._cur.execute("SELECT score FROM reviews")
        for score in iter(self._cur.fetchone, None):
            yield score

    def texts(self):
        """
        지도학습을 위해 전처리되고 벡터화한 전체 감상평 텍스트를 반환한다.
        Returns the full review texts
        """
        self._cur.execute("SELECT content FROM content")
        for text in iter(self._cur.fetchone, None):
            yield text

    def ids(self):
        """
        다른 감상평 메타데이터에 대한 조인을 활성화하는 감상평 ID들을 반환한다.
        Returns the review ids
        """
        self._cur.execute("SELECT reviewid FROM content")
        for idx in iter(self._cur.fetchone, None):
            yield idx
```
