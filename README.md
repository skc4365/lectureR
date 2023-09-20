# lectureR

[기초부터 코드를 따라하고 수정해볼 수 있는]
(https://www.w3schools.com/r/default.asp)

[2017 “R for Data Science”]
(https://r4ds.had.co.nz/index.html)

[2023  “R for Data Science”]
(https://r4ds.hadley.nz/)

[2023 “R Graphics Cookbook”]
(https://r-graphics.org/)

[구글문서_머신러닝이란?]
(https://developers.google.com/machine-learning/intro-to-ml/what-is-ml?hl=ko)

[2017 “Hands-On Machine Learning with R”]
(https://bradleyboehmke.github.io/HOML/)

---
![lib오류_경로설정](https://github.com/skc4365/lectureR/assets/50658047/0d56de64-538b-44d4-9d9a-b08e970a3cef)

-----
```
#참고: https://bradleyboehmke.github.io/HOML/linear-regression.html#final-thoughts
#선형회귀:Linear regression

#-----
#목표 : 부동산 속성을 사용하여 주택 판매 가격을 예측합니다.
#접근 : AmesHousing패키지 에서 제공 (Kuhn 2017 a )

#-----
#install.packages("caret")
#install.packages("vip")
#install.packages("AmesHousing")
library(tidyverse)
#모델링
library(caret)
#모델해석
library(vip)
#set.seed사용
library(rsample)
#AmesHousing::make_ames()사용
#library(AmesHousing)

#-----
#데이터준비
# Ames housing data
ames <- AmesHousing::make_ames()

#랜덤값 seed설정으로, 재실행시에 랜덤값이 같음
set.seed(123)
#데이터의 단일 이진 분할을 훈련세트와 테스트 세트로 생성한다.
#보관할 데이터의 비율(3/4)
split <- initial_split(ames, prop=0.7, strata="Sale_Price")

ames_train  <- training(split)
ames_test   <- testing(split)
names(ames_train)
head(ames_train$Sale_Price)

#-----
#모형적합_[판매가격, 면적]
model1 <- lm(Sale_Price ~ Gr_Liv_Area, data = ames_train)
str(model1)
class(model1)

#예측결과요약
summary(model1)

#-----
#모델의 추정계수는 15938.173, 109.667
#따라서, 평균 판매 가격이 지상 생활 공간이 1제곱피트 추가될 때마다 109.667만큼 증가한다고 추정한다.

#-----
#신뢰구간
confint(model1, level = 0.95)

#평균 판매 가격이 지상 생활 공간이 1제곱피트 추가 될 때마다 (104.920 ~ 114.4149)사이로 증가한다고 95% 신뢰도로 추정한다.

#-----
#모델정확도 평가
set.seed(123)
(cv_model1 <- train(
    form = Sale_Price ~ Gr_Liv_Area, 
    data = ames_train, 
    method = "lm",
    trControl = trainControl(method = "cv", number = 10)
))

#교차검증된 RMSE는 56644.76이다.
#10개의 CV에 대한 평균 RMSE다.
#결과: 이 모델의 예측은 평균적으로, 실제 판매할때의 가격에서 $56644.76 할인된다고 예측한다.

#----------
```


