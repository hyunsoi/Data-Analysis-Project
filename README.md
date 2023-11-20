# Data-Analysis-Project

## 1. Project Subject

Shopping Category Differences based on Age, Sex, and DATE.

## 2. Understand Dataset
``` python
df.head(10)
df.info()
```
 <img width="400" heigh="380" alt="스크린샷 2023-11-16 14 03 39" src="https://github.com/hyunsoi/Data-Analysis-Project/assets/102220333/50a589bd-a8be-4714-9fb7-326b01205cbe">
 <img width="400" heigh="380" alt="스크린샷 2023-11-16 14 05 00" src="https://github.com/hyunsoi/Data-Analysis-Project/assets/102220333/c714afa7-0f3c-4dba-904a-56e82d20bfc6">

## 3. Write Worksheets
|No|Question|
|:---:|:---:|
|1 | 성별에 따른 카드 사용량 상관관계|
|2 | 성별에 따른 카테고리의 상관관계|
|3 | 평일과 휴일의 건수 상관관계|
|4 | 성별에 따른 쇼핑 시간대의 상관관계|
|5 | 연령대에 따른 쇼핑 카테고리 상관관계|
|6 | 코로나 이전과 이후의 쇼핑 카테고리의 변화 |

## 4. Data Preprocessing

### Renaming Variation
```python
df = df.rename(
    columns = {'CRI_YM' : '연월',
               'TAG' : '카테고리'})
```
- CRI_YM → 연월
- TAG → 카테고리

### Preprocessing
```python
df['성별'] = np.where(df['성별'] == 'F', '여', '남')

df['연령대'] = np.where(df['연령대'] == 'A.2O대', 20,
            np.where(df['연령대'] == 'B.3O대', 30,
            np.where(df['연령대'] == 'C.4O대', 40,
            np.where(df['연령대'] == 'D.5O대', 50,
            np.where(df['연령대'] == 'E.60대이상', 60, df['연령대'])))))

df['시간대'] = np.where(df['시간대'] == 'A.02-06시', '02-06시', 
                np.where(df['시간대'] == 'B.06-10시', '06-10시', 
                np.where(df['시간대'] == 'C.10-14시', '10-14시', 
                np.where(df['시간대'] == 'D.14-18시', '14-18시', 
                np.where(df['시간대'] == 'E.18-22시', '18-22시',
                np.where(df['시간대'] == 'F.22-02시', '22-02시', df['시간대']))))))

```
- F, M → 여, 남
- 연령대 → 20 ~ 60대 이상
- 연월 → 연도

### Assign Derived Variation
```python
df['평균 쇼핑 시간대'] = np.where(df['시간대'] == 'A.02-06시', 4, 
                np.where(df['시간대'] == 'B.06-10시', 8, 
                np.where(df['시간대'] == 'C.10-14시', 12, 
                np.where(df['시간대'] == 'D.14-18시', 16, 
                np.where(df['시간대'] == 'E.18-22시', 20,
                np.where(df['시간대'] == 'F.22-02시', 0, df['시간대']))))))
```
- 평균 쇼핑 시간대 : 시간대의 평균값

### Check Category Type
```python
df[{'카테고리'}].drop_duplicates().reset_index(drop=True).T
df['카테고리'].drop_duplicates().reset_index(drop=True)
```
<img width="793" alt="스크린샷 2023-11-20 17 13 27" src="https://github.com/hyunsoi/Data-Analysis-Project/assets/102220333/e4051226-3b59-4c84-8478-95f62a238d79">
<img width="236" alt="스크린샷 2023-11-20 17 17 31" src="https://github.com/hyunsoi/Data-Analysis-Project/assets/102220333/48384e2c-e221-45f1-a0c7-ba14dc857b3d">


### Missing Value Check
```python
df['카테고리'].drop_duplicates().reset_index(drop=True)
```
 <img width="153" alt="스크린샷 2023-11-16 14 14 17" src="https://github.com/hyunsoi/Data-Analysis-Project/assets/102220333/b08453f3-1457-4403-b39b-5c6da8c027c5">

### Result
 <img width="503" alt="스크린샷 2023-11-17 12 19 50" src="https://github.com/hyunsoi/Data-Analysis-Project/assets/102220333/dc79434f-8b1a-4c6d-ae0d-3a717ec502b1">



## 5. Data Visualization

### 성별에 따른 카드 사용량의 상관관계
```python
df.groupby('성별') \
    .agg(sum_건수합계 = ('건수합계', 'sum')) #총 건수합계

df.groupby('성별') \
    .agg(mean_건수합계 = ('건수합계', 'mean')) #평균 건수합계

df.groupby('성별') \
    .agg(max_건수합계 = ('건수합계', 'max')) #최대 건수합계

df.groupby('성별') \
    .agg(min_건수합계 = ('건수합계', 'min')) #최소 건수합계
```
<img width="124" alt="스크린샷 2023-11-20 16 49 30" src="https://github.com/hyunsoi/Data-Analysis-Project/assets/102220333/9c834b01-0cc5-4d75-88ff-43c39a273e79">
<img width="124" alt="스크린샷 2023-11-20 16 50 39" src="https://github.com/hyunsoi/Data-Analysis-Project/assets/102220333/ff1cf2e3-367f-456d-a519-24ec12757380">
<img width="124" alt="스크린샷 2023-11-20 16 59 02" src="https://github.com/hyunsoi/Data-Analysis-Project/assets/102220333/2fcffa21-3fe3-4241-9260-44221aafd185">
<img width="124" alt="스크린샷 2023-11-20 16 59 11" src="https://github.com/hyunsoi/Data-Analysis-Project/assets/102220333/0e9c2dea-70e9-469d-8297-6ecddea3e53e">

### 성별에 따른 카테고리의 상관관계
``` python
# 성별과 카테고리 간의 교차표 생성
cross_tab = pd.crosstab(df['성별'], df['카테고리'])


# 세로로 출력
print(cross_tab.stack())
```
<img width="236" alt="스크린샷 2023-11-20 17 39 31" src="https://github.com/hyunsoi/Data-Analysis-Project/assets/102220333/6166a33a-ed22-4118-ac66-88bafec4bae4">

##### 데이터를 수집할 때 남녀별로 카테고리를 동일한 크기로 가져온 것처럼 보여짐. 
##### 따라서 이 데이터 파일에서는 단순히 성별에 따른 카테고리의 상관관계를 분석하기 어려움.

## 6. Conclusion
.
