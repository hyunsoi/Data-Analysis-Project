# Data-Analysis-Project

## 1. Project Subject

Shopping Category Differences based on Age, Sex,  Date and Time.

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
|1 | 성별과 연령대에 따른 카드 사용량의 관계|
|2 | 성별과 연령대에 따른 쇼핑 시간대의 관계|
|3 | 성별과 연령대에 따른 쇼핑 카테고리별 카드 사용량의 관계|
|4 | 평일과 휴일의 건수 상관관계|


## 4. Data Preprocessing

### Renaming Variation
```python
df = df.rename(
    columns = {'CRI_YM' : 'year',
               'TAG' : 'category',
               '건수합계' : '카드 사용량'})
```
- CRI_YM → year
- TAG → category
- 건수합계 → 카드 사용량
- 평일휴일 → 그대로
- 요일 → 그대로
- 시간대 → 그대로
- 성별 → 그대로

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
df['카테고리'].drop_duplicates().reset_index(drop=True)
```
<img width="236" alt="스크린샷 2023-11-20 17 17 31" src="https://github.com/hyunsoi/Data-Analysis-Project/assets/102220333/48384e2c-e221-45f1-a0c7-ba14dc857b3d">


### Missing Value Check
```python
df.isna().sum()
```
 <img width="153" alt="스크린샷 2023-11-16 14 14 17" src="https://github.com/hyunsoi/Data-Analysis-Project/assets/102220333/b08453f3-1457-4403-b39b-5c6da8c027c5">

### Result
 <img width="503" alt="스크린샷 2023-11-17 12 19 50" src="https://github.com/hyunsoi/Data-Analysis-Project/assets/102220333/dc79434f-8b1a-4c6d-ae0d-3a717ec502b1">



## 5. Data Visualization

### 성별과 연령대에 따른 카드 사용량의 상관관계
```python
df2.groupby(['성별','연령대']) \
    .agg(mean_카드사용량 = ('카드 사용량', 'count'))

df2.query("성별 == 'Male'") \
    .groupby(['성별','연령대']) \
    .agg(mean_카드사용량 = ('카드 사용량', 'count')) \
    .sort_values('mean_카드사용량', ascending = False)

df2.query("성별 == 'Female'") \
    .groupby(['성별','연령대']) \
    .agg(mean_카드사용량 = ('카드 사용량', 'count')) \
    .sort_values('mean_카드사용량', ascending = False)

sns.set(style="white", palette="Set2")
plt.rcParams['font.family'] = 'AppleGothic'
plt.figure(figsize=(10, 6))
sns.barplot(data = df2, x = '연령대', y = '카드 사용량') 
plt.show()
```
<img width="200" alt="스크린샷 2023-11-22 17 06 58" src="https://github.com/hyunsoi/Data-Analysis-Project/assets/102220333/96161937-5734-4aa7-a63f-add8894a8d52">
<img width="200" alt="스크린샷 2023-11-22 17 07 47" src="https://github.com/hyunsoi/Data-Analysis-Project/assets/102220333/f7af4e90-6da0-4e1d-89ea-f851700d3d9d">
<img width="200" alt="스크린샷 2023-11-22 17 08 04" src="https://github.com/hyunsoi/Data-Analysis-Project/assets/102220333/f5f960ff-2d49-4bd4-994a-2b4311123336">
<img width="390" alt="스크린샷 2023-11-22 17 17 29" src="https://github.com/hyunsoi/Data-Analysis-Project/assets/102220333/28efb2af-f178-498b-935c-94da66bafe95">

### 셩별과 연령대에 따른 쇼핑 시간대의 관계



### 3. 성별과 연령대에 따른 카테고리별 카드 사용량의 관계



## 6. Conclusion
.
