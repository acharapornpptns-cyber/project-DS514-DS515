# project-DS514-515

โครงงานนี้เป็นส่วนหนึ่งของรายวิชา DS514/DS515 Data Science จากข้อมูลชุดนี้เป็นข้อมูลการขายสินค้า/บริการของบริษัทแห่งหนึ่ง โดยมีวัตถุประสงค์หลักในวิเคราะห์หรือทำนายช่องทางการขายที่เหมาะสม เพื่อช่วยให้ทีมขายเลือกช่องทางได้ตรงกับประเภทสินค้าและราคาที่เสนอได้

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

### 🧠 Model Training: K-Nearest Neighbors (KNN)

การสร้างโมเดล KNN และใช้เทคนิค **GridSearchCV** เพื่อค้นหาค่า Hyperparameter ที่เหมาะสมที่สุด (`n_neighbors`) โดยกำหนดค่า k ตั้งแต่ 3 ถึง 25

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

### Model Performance Evaluation
Here is the classification report for the model:

| Class | Precision | Recall | F1-Score | Support |
| :--- | :---: | :---: | :---: | :---: |
| **0** | 0.40 | 0.52 | 0.45 | 62 |
| **1** | 0.85 | 0.83 | 0.84 | 84 |
| **2** | 0.56 | 0.45 | 0.50 | 82 |
| | | | | |
| **Accuracy** | | | **0.61** | **228** |
| **Macro Avg** | 0.60 | 0.60 | 0.60 | 228 |
| **Weighted Avg**| 0.62 | 0.61 | 0.61 | 228 |

> **Note:** The model performs best on **Class 1** (F1-score: 0.84) but struggles with **Class 0** (F1-score: 0.45).

## การประเมินผลโมเดล (Evaluation)
* Accuracy
* Precision
* Recall
* F1-Score
* Confusion Matrix
* Classification Report

## Conclusion
* ทั้งสองโมเดลมีความแม่นยำเฉลี่ยใกล้เคียงกัน (~60%) <br>
* Class 0 (DirectSales) Logistic Regression ให้ Recall สูงถึง 68% ดีกว่า KNN <br>
* Class 1 (Organic) ถูกทำนายได้ดีที่สุดในทั้งสองโมเดล โดยมี Recall 83–90% <br>
* Class 2 (TeleSales) คือ class ที่โมเดลทำนายได้แย่ที่สุด (ไม่ถึง 40–50%) <br>
* โมเดล Logistic Regression ให้ค่าความแม่ยำดีกว่า และค่าแต่ละ class ทำนายถูกได้มากกว่า KNN <br>
* โมเดลสามารถทำนาย Organic ได้ดี แต่ยังไม่แม่นยำสำหรับ TeleSales อาจจะเพราะข้อมูลซับซ้อนเกินไป <br>

