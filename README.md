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

### 1. 성별과 연령대에 따른 카드 사용량의 상관관계
```python
df2_A = df2.groupby(['성별','연령대']) \
        .agg(평균_카드사용량 = ('카드 사용량', 'mean'))

df2_A
```
<img width="190" height = "370" alt="스크린샷 2023-11-29 19 33 48" src="https://github.com/hyunsoi/Data-Analysis-Project/assets/102220333/6c3d4b87-6c02-4380-b7af-ef2476e7f5c9">
.
<img width="585" height = "370" alt="스크린샷 2023-11-29 19 34 26" src="https://github.com/hyunsoi/Data-Analysis-Project/assets/102220333/209594a8-d0bc-4151-878e-a72f495e31a2">
<img width="450" height = "370 alt="스크린샷 2023-11-29 20 03 13" src="https://github.com/hyunsoi/Data-Analysis-Project/assets/102220333/49443825-a6b1-433f-9dd2-2190ebb3a69a">
<img width="450" height = "370 alt="스크린샷 2023-11-29 20 03 28" src="https://github.com/hyunsoi/Data-Analysis-Project/assets/102220333/9b6e1641-19dc-4444-9e2d-1b1104f66917">



### 2. 성별과 연령대에 따른 쇼핑 시간대의 상관관계
```python
# 남자 데이터에 대한 그래프


plt.figure(figsize=(9, 7))  # 전체 그림 크기 설정

# subplot 1 (위쪽)
plt.subplot(2, 2, 1)
plt.title('20대')

plt.ylim(0, 3000)
sns.lineplot(data = df3_20, x = '평균 쇼핑 시간대', y = '카드 사용량', hue = '성별', ci =None, palette = 'Set1')

# 여자 데이터에 대한 그래프
#max_value_row_F = df4_F.loc[df4_F['여자_평균_카드사용량'].idxmax()]

# subplot 2 (아래쪽)
plt.subplot(2, 2, 2)
plt.title('30대')

plt.ylim(0, 3000)
sns.lineplot(data = df3_30, x = '평균 쇼핑 시간대', y = '카드 사용량', hue = '성별', ci =None, palette = 'Set1')

# subplot 1 (위쪽)
plt.subplot(2, 2, 3)
plt.title('40대')

plt.ylim(0, 3000)
sns.lineplot(data = df3_40, x = '평균 쇼핑 시간대', y = '카드 사용량', hue = '성별', ci =None, palette = 'Set1')

# 여자 데이터에 대한 그래프
#max_value_row_F = df4_F.loc[df4_F['여자_평균_카드사용량'].idxmax()]

# subplot 2 (아래쪽)
plt.subplot(2, 2, 4)
plt.title('50대')

plt.ylim(0, 3000)
sns.lineplot(data = df3_50, x = '평균 쇼핑 시간대', y = '카드 사용량', hue = '성별', ci =None, palette = 'Set1')

plt.tight_layout()
plt.show()
```
![image](https://github.com/hyunsoi/Data-Analysis-Project/assets/102220333/f9468b01-7e74-44c8-a75b-dcaeb930ac6d)

```python
plt.figure(figsize=(10, 4))  # 전체 그림 크기 설정

# subplot 1 (위쪽)
plt.subplot(1, 2, 1)
plt.title('남자 평균 쇼핑 시간대')

sns.lineplot(data = df3_M, x = '평균 쇼핑 시간대', y = '카드 사용량', hue = '연령대', ci = None, palette = 'muted')

# 여자 데이터에 대한 그래프
#max_value_row_F = df4_F.loc[df4_F['여자_평균_카드사용량'].idxmax()]

# subplot 2 (아래쪽)
plt.subplot(1, 2, 2)
plt.title('여자 평균 쇼핑 시간대')

sns.lineplot(data=df3_F, x='평균 쇼핑 시간대', y='카드 사용량', hue='연령대', ci=None, palette = 'muted')
```

<img width="863" alt="스크린샷 2023-11-29 20 28 00" src="https://github.com/hyunsoi/Data-Analysis-Project/assets/102220333/b637c6cf-45e5-4b89-8f1d-eb8b8fc5f346">

### 3. 성별과 연령대에 따른 카테고리별 카드 사용량의 관계
```python
 #df4_M_20 = 20대 남성
 #df4_M_30 = 30대 남성
 #df4_M_40 = 40대 남성
 #df4_M_50 = 50대 남성

df_combined = pd.concat([df4_M_20, df4_M_30, df4_M_40, df4_M_50], axis=1)

df_combined
```
![image](https://github.com/hyunsoi/Data-Analysis-Project/assets/102220333/7b12333a-f177-45b3-af4c-8c81be10b4b3)
<div>
 <h4> 연령대별 남자 카테고리별 카드 사용량 </h5>
 <img src ="https://github.com/hyunsoi/Data-Analysis-Project/assets/102220333/a2dee7eb-630f-44d6-bafd-1b5f43168101">
</div>
<div>
 <h4"> 연령대별 여자 카테고리별 카드 사용량</h5>
 <img src="https://github.com/hyunsoi/Data-Analysis-Project/assets/102220333/8b6f93de-f028-4ed3-ac59-ccd821fa049c">
</div>





## 6. Conclusion
.
