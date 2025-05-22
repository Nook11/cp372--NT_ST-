# 📊 Nike Global Sales Analysis (2024) Review
วิเคราะห์ยอดขายสินค้า Nike ปี 2024 โดยเน้นการเพิ่มยอดขายผ่านการสำรวจเชิงลึกด้วย Tableau การคำนวณกำไร และการวิเคราะห์ข้อมูล
- Analysis Tabbleau [Download](./Data_Set_nike_sales_2024.xlsx)
- Presentation Video [Download](./Data_Set_nike_sales_2024.xlsx)
- Doccument ฉบับเต็ม [Download](./Data_Set_nike_sales_2024.xlsx)
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
# การสำรวจข้อมูลเบื้องต้น (Exploratory Data Analysis, EDA)
### 📊 1. รายได้รวมตามภูมิภาค
วิเคราะห์รายได้รวมของแต่ละภูมิภาค

- **จีน** มียอดขายสูงที่สุด
- รองลงมาคือ **ญี่ปุ่น**
![image](https://github.com/user-attachments/assets/51fab944-c38b-4dcd-993f-ab027b6bf62b)

---

### 📆 2. รายได้รวมของหมวดหมู่สินค้ารายเดือน
ช่วยให้เข้าใจช่วงเวลายอดขายสูงของแต่ละหมวด

- Apparel → พีคใน March, June, July  
- Equipment → พีคใน May, November  
- Footwear → พีคใน December, February  

![image](https://github.com/user-attachments/assets/cd30a31b-4f5f-4c49-a761-2166f3211c13)


---

### 🧥 3. รายได้รวมตามประเภทสินค้า
เรียงจากสินค้าที่ทำรายได้มากที่สุด

- Outerwear และ Accessories สร้างรายได้ดี


![image](https://github.com/user-attachments/assets/180322e5-56bb-42c8-acdd-16e76986803b)


---

### 📈 4. ประสิทธิภาพหมวดหมู่สินค้า
วิเคราะห์จากจำนวน Units Sold

- *Equipment* และ *Apparel* ขายดีที่สุด
- *Footwear* มีจำนวนสินค้ามากแต่ยอดขายน้อย 

![image](https://github.com/user-attachments/assets/73fe57cb-d34a-4eed-b9f2-4ef08dcdc2b1)


---

### 🌐 5. Online Units vs Offline Units
วิเคราะห์พฤติกรรมการซื้อของลูกค้า

- ทุกหมวดสินค้า: ยอดขาย **ออนไลน์** มากกว่าหน้าร้าน

![image](https://github.com/user-attachments/assets/0a93f615-ecf7-4e6c-b59c-caa1792ac98d)


---

### 💰 6. ยอดขายตามระดับราคา (Price Tier)
พฤติกรรมลูกค้าแบ่งตามระดับราคา

- Budget / Mid-Range / Premium


![image](https://github.com/user-attachments/assets/7de52e14-6854-421e-94f6-7caf404e6170)


---

🧾 7. กำไรของแต่ละหมวดสินค้า 
- Equipment เป็นหมวดสินค้าที่สร้างกำไรได้มากที่สุด

- Apparel ตามมาเป็นอันดับสอง
- Footwear มีกำไรต่ำที่สุด แม้จะมีหลายผลิตภัณฑ์ย่อย

![image](https://github.com/user-attachments/assets/920292da-8f8e-430f-99e7-484abf4f0ee4)

---
# การวิเคราะห์เชิงลึก (In-Depth Analysis)
### ประเภทของสินค้าที่ขายดีในญี่ปุ่น มียอดขายเท่าใดและทำไมสินค้าตัวนี้ถึงมียอดขายที่มาก


![image](https://github.com/user-attachments/assets/4ef50e9e-0912-4786-8db0-993ace7c00d1)

### สินค้าที่ขายได้ดีตลอดทั้งปี คือ Rain Jacket เนื่องจากประเทศญี่ปุ่นมี 


### ✅ เหตุผลที่ยอดขายสูงตลอดทั้งปี:


1. **🌧️ ภูมิอากาศและฤดูกาล**
   - ญี่ปุ่นมีฤดูฝน “ซึยุ” (มิ.ย. – กลาง ก.ค.) ฝนตกแทบทุกวัน
   - มีฤดูพายุไต้ฝุ่น (ส.ค. – ก.ย.)
   - ส่งผลให้มีความต้องการใช้งานเสื้อกันฝนตลอดปี

2. **🚆 วัฒนธรรมการเดินทางด้วยขนส่งสาธารณะ**
   - คนญี่ปุ่นนิยมใช้รถไฟหรือเดิน
   - เสื้อกันฝน **สะดวกกว่าร่ม** ไม่เกะกะเวลาเดินหรือขึ้นรถไฟ

3. **🧳 สังคมที่ให้ความสำคัญกับการเตรียมพร้อม**
   - คนญี่ปุ่นมัก **เตรียมตัวล่วงหน้า**
   - ร้านค้ามีเสื้อกันฝนวางจำหน่ายตลอดปี ทำให้เข้าถึงง่าย

---



