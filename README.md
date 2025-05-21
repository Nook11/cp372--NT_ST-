# 📊 Nike Global Sales Analysis (2024)
วิเคราะห์ยอดขายสินค้า Nike ปี 2024 โดยเน้นการเพิ่มยอดขายผ่านการสำรวจเชิงลึกด้วย Tableau การคำนวณกำไร และการวิเคราะห์ข้อมูล
## Data Set
Nike Global Sales ข้อมูลปี 2024 - [Data Set Download](./Data_Set_nike_sales_2024.xlsx)
## Problem Statement
* Nike จะมีการจำหน่ายสินค้าในหลากหลายประเทศ เเละหลากหลายประเภท เเต่สินค้าในหมวดหมู่Footwear ยอดขายที่ต่ำมากอย่างเห็นได้ชัด ซึ่งควรจะต้องเป็นสินค้าหลักเมื่อเทียบกับหมวดอื่นๆ
## Business Objectives
* การเพิ่มยอดขายสินค้าหมวดหมู่Footwaerเพิ่มขึ้นอย่างน้อย 5% ภายใน6เดือน โดยศึกษาจากยอดขายของปี 2024
## Key Stakeholders
* ฝ่ายการตลาดและฝ่ายขาย
## Success Metrics
* การเติบโตของเเต่ละไตรมาส
* ยอดขายของเเต่ละเดือน
# การเตรียมข้อมูล (Data Preparation) 
### 1.การทำความสะอาดข้อมูล (Data Cleaning) :
ขั้นตอน: โหลดข้อมูล: ใช้ pandas.read_csv() เพื่ออ่านไฟล์  nike_sales_2024_pj.csv
* ตวรจสอบค่าที่ว่าง
```python
df.isnull().sum()
```
* การลบข้อมูลซ้ำ
```python
df = df.drop_duplicates()
```
### 2.การสร้างคุณลักษณะใหม่ (Feature Engineering):
1️⃣ `Total_Orders` (จำนวนคำสั่งซื้อรวมต่อเดือน)
- ทำการนับจำนวนคำสั่งซื้อ (จำนวนแถว) ในแต่ละเดือน
- เพิ่มเป็นฟีเจอร์ใหม่ใน DataFrame
- ใช้สำหรับวิเคราะห์แนวโน้มจำนวนออเดอร์ในแต่ละช่วงเวลา

```python
# สร้างตารางสรุปคำสั่งซื้อทั้งหมดในแต่ละเดือน
monthly_orders = df['Month'].value_counts().rename_axis('Month').reset_index(name='Total_Orders')
df = df.merge(monthly_orders, on='Month', how='left')
df[['Month', 'Total_Orders']].drop_duplicates().sort_values('Month')
```
### สร้างโดยใช้ Create Calculated Field
![image](https://github.com/user-attachments/assets/004f0d76-c507-45f8-8e49-4c7f5a694b9a)

2️⃣ Profit (กำไร)
* คำนวณกำไรรวมของสินค้าแต่ละรายการ
* สมมุติฐาน: ต้นทุนสินค้า = 60% ของราคาขายปลีก → กำไรต่อหน่วย = 40%
##### สูตร: 
```python
Profit = (Retail Price * 0.4) * Units Sold
```
3️⃣ Online_Units (จำนวนสินค้าที่ขายได้ทางออนไลน์)
* ใช้ Online_Sales_Percentage คูณกับยอดขายทั้งหมด (Units_Sold)
##### สูตร:
```python
Online Units = Units Sold * (Online Sales Percentage / 100)
```
4️⃣ Offline_Units (จำนวนสินค้าที่ขายได้ทางออฟไลน์)
คำนวณโดยหาส่วนที่ไม่ใช่ยอดขายออนไลน์

##### สูตร:
```python
Offline Units = Units_Sold * (1 - (Online Sales Percentage / 100))
```


