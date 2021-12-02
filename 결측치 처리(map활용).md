## T1-3. 결측치 처리(map활용)

주어진 데이터에서 결측치가 80%이상 되는 컬럼은(변수는) 삭제하고, 80% 미만인 결측치가 있는 컬럼은 'city'별 중앙값으로 값을 대체하고 'f1'컬럼의 평균값을 출력하세요!

[주의]
- map 함수는 Series에서 사용 가능하다.
- df.groupby('city')로 그룹별로 분류,통계 가능
```python3
# 라이브러리 , 데이터 불러오기
import pandas as pd
import numpy as np

df = pd.read_csv('../input/bigdatacertificationkr/basic1.csv')

# 결측치 찾기
df.isnull().sum() # f1과 f3이 결측치가 존재함을 확인

# 결측치가 80퍼센트 되는 컬럼 찾고 삭제
(df.isnull().sum())/(df.shape[0]) * 100 >= 80 # f3이 해당하는 컬럼임을 확인

df.drop(['f3'],axis=1)

# city확인
df['city'].unique() #서울,부산,대구,경기

# city별 중앙값 찾기
s = df[df['city'] == '서울']['f1'].median()
b = df[df['city'] == '부산']['f1'].median()
d = df[df['city'] == '대구']['f1'].median()
g = df[df['city'] == '경기']['f1'].median()

#또는
s,b,d,g = df.groupby('city')['f1'].median()

# 나머지 컬럼의 결측치를 지역별 중앙값으로 채우기
d = {'서울':s, '부산':b, '대구':d, '경기':g}
df['f1'].fillna(df['city'].map(d))

# f1의 평균값 구하기
print(df['f1'].mean())
```
