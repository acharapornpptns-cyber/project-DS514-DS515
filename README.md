# Project Final DS514-515
โครงการนี้เป็นส่วนหนึ่งของรายวิชา **DS512/DS513 Data Analytics** และ **DS514/DS515 Data Science** โดยมีวัตถุประสงค์หลักเพื่อวิเคราะห์ข้อมูลการขายสินค้า/บริการของบริษัท A และสร้างโมเดล Machine Learning เพื่อทำนาย "ช่องทางการขายที่เหมาะสม" (Sales Channel) ช่วยให้ทีมขายเลือกช่องทางได้ตรงกับประเภทสินค้าและราคาที่เสนอได้

## Data Preprocessing
* เปลี่ยนประเภทของข้อมูล
* ลบช่องว่างของข้อมูล
* เพิ่มคอลัมน์อายุ
* ตัดข้อมูลที่ไม่ถูกต้องออก (ผู้ที่อายุน้อยกว่า 14 ปี)

## Imbalance Data and Encoding
* จัดการข้อมูล Imbalance โดยการสุ่มข้อมูล (เลือกจำนวนข้อมูลจากจำนวนกลุ่มที่น้อยที่สุด)
* Encode ข้อมูลที่เป็น Category

## Target and Feature Selection 
Target :
* Sales_Channel

Feature : 
* Package_Type
* Price_After_Discount
* Price
* Subscription_Type
* Package_Group
* Age

## Model และ การปรับ Hyperparameters
* K-Nearest Neighbors
* Logistic Regression
  
ตัวอย่างโมเดล :

```python
from sklearn.neighbors import KNeighborsClassifier
from sklearn.model_selection import GridSearchCV

# Initialize the model
knn2 = KNeighborsClassifier()

# Define the parameter grid
param_grid = {'n_neighbors': [3, 5, 7, 9, 11, 13, 15, 17, 19, 21, 25]}

# Set up GridSearchCV with 5-fold Cross Validation
grid_search = GridSearchCV(knn2, param_grid, cv=5, scoring='accuracy')

# Fit the model
grid_search.fit(X_train_scaled2, y_train2)

# Display best parameters
print("Best parameters : ", grid_search.best_params_)

# Predict using the best estimator
best_knn = grid_search.best_estimator_
y_predict_train2 = best_knn.predict(X_train_scaled2)
y_predict_test2 = best_knn.predict(X_test_scaled2)
```
## การประเมินผลโมเดล (Evaluation)
* Accuracy
* Precision
* Recall
* F1-Score
* Confusion Matrix
* Classification Report

ตัวอย่างผลลัพธ์ :



| Class | Precision | Recall | F1-Score | Support |
| :--- | :---: | :---: | :---: | :---: |
| **0** | 0.40 | 0.52 | 0.45 | 62 |
| **1** | 0.85 | 0.83 | 0.84 | 84 |
| **2** | 0.56 | 0.45 | 0.50 | 82 |
| | | | | |
| **Accuracy** | | | **0.61** | **228** |
| **Macro Avg** | 0.60 | 0.60 | 0.60 | 228 |
| **Weighted Avg**| 0.62 | 0.61 | 0.61 | 228 |

## Conclusion
* ทั้งสองโมเดลมีความแม่นยำเฉลี่ยใกล้เคียงกัน (~60%)
* Class 0 (DirectSales) Logistic Regression ให้ Recall สูงถึง 68% ดีกว่า KNN
* Class 1 (Organic) ถูกทำนายได้ดีที่สุดในทั้งสองโมเดล โดยมี Recall 83–90%
* Class 2 (TeleSales) คือ class ที่โมเดลทำนายได้แย่ที่สุด (ไม่ถึง 40–50%)
* โมเดล Logistic Regression ให้ค่าความแม่ยำดีกว่า และค่าแต่ละ class ทำนายถูกได้มากกว่า KNN
* โมเดลสามารถทำนาย Organic ได้ดี แต่ยังไม่แม่นยำสำหรับ TeleSales อาจจะเพราะข้อมูลซับซ้อนเกินไป

