# Healthcare Patient Monitoring Hub

A comprehensive interactive dashboard demonstrating mastery of database application development using Python, SQLite3, and Jupyter Notebook. This project showcases the complete data lifecycle from schema design to advanced analytics and interactive visualization.

## 📋 Overview

This project implements a **Healthcare Patient Monitoring Hub** - an interactive dashboard for hospital patient monitoring that includes:

- **Multi-table relational database** with 4 related tables
- **Batch data generation** with 15,000+ realistic health records
- **Advanced SQL queries** including window functions, JOINs, and aggregations
- **Interactive visualizations** with real-time filtering capabilities

## 🎯 Features

### 1. Database Design
- **4 Related Tables**: Doctors, Patients, VitalsLog, Medications
- **Foreign Key Relationships**: Ensures referential integrity
- **Data Constraints**: Input validation and data quality checks
- **Performance Indexes**: Optimized queries for large datasets

### 2. Batch Data Simulation
- **1,000 Patients** with realistic demographics
- **50 Doctors** across 9 specialties with shift schedules
- **15,000 Vital Sign Records** with time-series trends
- **5,000 Medication Prescriptions** with dosage information

### 3. Advanced SQL Queries

#### Window Functions
- `ROW_NUMBER()` to identify most recent vital signs per patient
- `LAG()` to calculate changes from previous readings
- Moving averages for trend analysis

#### Aggregations
- `AVG()`, `COUNT()`, `SUM()` for statistical analysis
- `GROUP BY` with complex conditions
- `HAVING` clauses for filtered aggregations

#### Multi-Table JOINs
- 3-way JOINs between Patients, Doctors, and VitalsLog
- LEFT JOINs for comprehensive doctor workload analysis
- Subqueries for nested calculations

### 4. Interactive Dashboard

The dashboard provides:
- **Patient Recovery Trends**: Multi-line charts showing heart rate, temperature, and oxygen saturation over time
- **Medication Usage Analysis**: Bar charts of most prescribed medications
- **High-Risk Patient Alerts**: Automatic identification of patients with elevated heart rates
- **Shift Workload Distribution**: Visualization of busiest hours for medical staff

#### Interactive Controls
- Patient selector dropdown
- Heart rate threshold slider (80-120 bpm)
- Date range filter (0-90 days back)
- Specialty filter for doctor analysis

## 🚀 Getting Started

### Prerequisites
- Python 3.7 or higher
- Jupyter Notebook or Google Colab

### Installation

1. **Clone the repository**:
   ```bash
   git clone https://github.com/inhacollab/database-management.git
   cd database-management
   ```

2. **Open the notebook**:
   - **Google Colab**: Upload `healthcare_dashboard.ipynb`
   - **Local Jupyter**: 
     ```bash
     jupyter notebook healthcare_dashboard.ipynb
     ```

3. **Run all cells sequentially**:
   - Cell 1: Installs required packages
   - Cell 2: Imports libraries
   - Cell 3: Creates database schema
   - Cell 4: Generates and inserts mock data
   - Cell 5: Executes advanced SQL queries
   - Cell 6: Launches interactive dashboard

## 📊 Database Schema

### Doctors Table
```sql
CREATE TABLE Doctors (
    doctor_id INTEGER PRIMARY KEY,
    name TEXT NOT NULL,
    specialty TEXT NOT NULL,
    shift_start TEXT NOT NULL,
    shift_end TEXT NOT NULL,
    years_experience INTEGER
)
```

### Patients Table
```sql
CREATE TABLE Patients (
    patient_id INTEGER PRIMARY KEY,
    name TEXT NOT NULL,
    age INTEGER CHECK(age >= 0 AND age <= 120),
    gender TEXT CHECK(gender IN ('Male', 'Female', 'Other')),
    admission_date TEXT NOT NULL,
    diagnosis TEXT,
    doctor_id INTEGER NOT NULL,
    FOREIGN KEY (doctor_id) REFERENCES Doctors(doctor_id)
)
```

### VitalsLog Table
```sql
CREATE TABLE VitalsLog (
    vitals_id INTEGER PRIMARY KEY,
    patient_id INTEGER NOT NULL,
    timestamp TEXT NOT NULL,
    heart_rate INTEGER CHECK(heart_rate >= 0 AND heart_rate <= 300),
    blood_pressure TEXT,
    temperature REAL CHECK(temperature >= 30.0 AND temperature <= 45.0),
    oxygen_saturation INTEGER CHECK(oxygen_saturation >= 0 AND oxygen_saturation <= 100),
    FOREIGN KEY (patient_id) REFERENCES Patients(patient_id)
)
```

### Medications Table
```sql
CREATE TABLE Medications (
    med_id INTEGER PRIMARY KEY,
    patient_id INTEGER NOT NULL,
    medication_name TEXT NOT NULL,
    dosage TEXT NOT NULL,
    frequency TEXT NOT NULL,
    administered_date TEXT NOT NULL,
    prescribed_by INTEGER,
    FOREIGN KEY (patient_id) REFERENCES Patients(patient_id),
    FOREIGN KEY (prescribed_by) REFERENCES Doctors(doctor_id)
)
```

## 🔍 Key SQL Queries

### High-Risk Patient Detection
Uses window functions to identify patients with elevated heart rates:
```sql
WITH RecentVitals AS (
    SELECT 
        patient_id,
        heart_rate,
        ROW_NUMBER() OVER (PARTITION BY patient_id ORDER BY timestamp DESC) as rn
    FROM VitalsLog
)
SELECT 
    p.patient_id,
    p.name,
    AVG(rv.heart_rate) as avg_heart_rate
FROM Patients p
JOIN RecentVitals rv ON p.patient_id = rv.patient_id
WHERE rv.rn <= 3
GROUP BY p.patient_id, p.name
HAVING AVG(rv.heart_rate) > 100
```

### Busiest Shift Hours
Multi-table JOIN with time-based aggregation:
```sql
SELECT 
    strftime('%H', v.timestamp) as hour_of_day,
    COUNT(*) as total_checks,
    COUNT(DISTINCT p.patient_id) as unique_patients
FROM VitalsLog v
JOIN Patients p ON v.patient_id = p.patient_id
JOIN Doctors d ON p.doctor_id = d.doctor_id
GROUP BY hour_of_day
ORDER BY total_checks DESC
```

## 📈 Visualization Examples

The dashboard generates:
1. **Line Charts**: Time-series vital signs with threshold indicators
2. **Bar Charts**: Medication frequency and shift workload
3. **Interactive Filters**: Real-time data exploration
4. **Statistical Summaries**: Min, max, and average calculations

## 🎨 Technologies Used

- **Database**: SQLite3
- **Data Processing**: Pandas, NumPy
- **Visualization**: Plotly, Matplotlib, Seaborn
- **Interactive UI**: ipywidgets
- **Development Environment**: Jupyter Notebook / Google Colab

## 📝 Code Structure

The notebook is organized into 6 main cells:

1. **Package Installation**: All required dependencies
2. **Library Imports**: Organized imports with documentation
3. **Schema Creation**: Modular database setup function
4. **Data Generation**: Batch processing with realistic patterns
5. **SQL Queries**: Advanced analytics functions
6. **Interactive Dashboard**: Multi-panel visualization system

## 🎓 Educational Value

This project demonstrates:
- **Database Design**: Normalization, relationships, constraints
- **SQL Proficiency**: Complex queries, window functions, subqueries
- **Data Engineering**: Batch processing, data validation
- **Python Programming**: Modular functions, error handling
- **Data Visualization**: Interactive charts, user interfaces
- **Best Practices**: Code documentation, parameterized queries

## 🤝 Contributing

This is an educational project. Suggestions for improvements are welcome!

## 📄 License

This project is created for educational purposes.

## 👤 Author

Created as part of the Database Application Development course.

## 🔗 Repository

[https://github.com/inhacollab/database-management](https://github.com/inhacollab/database-management)

---

**Note**: This notebook is designed to run in Google Colab or local Jupyter environments. All packages are installed automatically in Cell 1.
