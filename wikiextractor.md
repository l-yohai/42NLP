# WIKIEXTRACTOR

## USAGE
In bash
```Shell
git clone "https://github.com/attardi/wikiextractor.git"  
python WikiExtractor.py kowiki-latest-pages-articles.xml.bz2  

copy AA디렉토리의 경로\wiki* wikiAA.txt
copy AB디렉토리의 경로\wiki* wikiAB.txt
copy AC디렉토리의 경로\wiki* wikiAC.txt
copy AD디렉토리의 경로\wiki* wikiAD.txt
copy AE디렉토리의 경로\wiki* wikiAE.txt
copy AF디렉토리의 경로\wiki* wikiAF.txt
copy 현재 디렉토리의 경로\wikiA* wiki_data.txt
```

In Jupyer Notebook
```python
f = open('wiki_data.txt 파일의 경로', encoding="utf8")
# 예를 들어 위도우 바탕화면에서 작업한 저자의 경우
# f = open(r'C:\Users\USER\Desktop\wiki_data.txt', encoding="utf8")

i=0
while True:
    line = f.readline()
    if line != '\n':
        i=i+1
        print("%d번째 줄 :"%i + line)
    if i==5:
        break 
f.close()
```
```Python
from konlpy.tag import Okt  
okt=Okt()
fread = open('wiki_data.txt 파일의 경로', encoding="utf8")
# 파일을 다시 처음부터 읽음.
n=0
result = []

while True:
    line = fread.readline() #한 줄씩 읽음.
    if not line: break # 모두 읽으면 while문 종료.
    n=n+1
    if n%5000==0: # 5,000의 배수로 While문이 실행될 때마다 몇 번째 While문 실행인지 출력.
        print("%d번째 While문."%n)
    tokenlist = okt.pos(line, stem=True, norm=True) # 단어 토큰화
    temp=[]
    for word in tokenlist:
        if word[1] in ["Noun"]: # 명사일 때만
            temp.append((word[0])) # 해당 단어를 저장함

    if temp: # 만약 이번에 읽은 데이터에 명사가 존재할 경우에만
      result.append(temp) # 결과에 저장
fread.close()

print('총 샘플의 개수 : {}'.format(len(result))
```
```Python
from gensim.models import Word2Vec
model = Word2Vec(result, size=100, window=5, min_count=5, workers=4, sg=0)

model_result1 = model.wv.most_similar("대한민국")
print(model_result1)
model_result2 = model.wv.most_similar("어벤져스")
print(model_result2)
model_result3 = model.wv.most_similar("반도체")
print(model_result3)
```

#### 모델 다운로드 경로 : https://drive.google.com/file/d/B7XkCwpI5KDYNlNUTTlSS21pQmM/edit

```Python
import gensim

# 구글의 사전 훈련된 Word2Vec 모델을 로드합니다.
model = gensim.models.KeyedVectors.load_word2vec_format('GoogleNews-vectors-negative300.bin 파일 경로', binary=True)  
print(model.vectors.shape) # 모델의 크기 확인
print (model.similarity('this', 'is')) # 두 단어의 유사도 계산하기
print (model.similarity('post', 'book'))
print(model['book']) # 단어 'book'의 벡터 출력
```

```Python
import gensim
model = gensim.models.Word2Vec.load('ko.bin 파일의 경로')
result = model.wv.most_similar("강아지")
print(result)
```