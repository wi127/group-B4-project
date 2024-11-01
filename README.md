## GROUP B4 colaborators : 
_- 22980 NKUSI William_
_- 25048 NTWALI Deus_
_- 24154 MUGABO Patience_
_- 23752 IRAKOZE Peace Arlaine_
_- 24104 DUSABEYESU Sangwa Fabrice_
_- 26500 SHEJA N M Yves_
_- 26685 MANIRABONA Hirwa Patience_
   
 # group assingment 
 
 **Scenario:** You are tasked with managing employee attendance records in a company database. Create a PL/SQL program that includes 
a procedure.

# Introduction
This project contains a PL/SQL procedure designed to calculate monthly attendance statistics for employees. The procedure, Calculate_Attendance_Stats, takes the month and year as parameters and calculates the total days present and absent for each employee, along with their attendance percentage. The goal of this project is to streamline attendance reporting by providing a quick summary of each employee’s attendance in a specified month.

# Procedure Code

```sql
CREATE OR REPLACE PROCEDURE Calculate_Attendance_Stats ( 
    p_month NUMBER,
    p_year NUMBER
) IS
    v_total_presents NUMBER := 0;
    v_total_absents NUMBER := 0;
    v_days_in_month NUMBER := 0;
    v_attendance_percentage NUMBER := 0;

BEGIN
    SELECT TO_NUMBER(TO_CHAR(LAST_DAY(TO_DATE(p_year || '-' || p_month || '-01', 'YYYY-MM-DD')), 'DD'))
    INTO v_days_in_month
    FROM dual;

    FOR emp IN (SELECT employee_id, first_name, last_name FROM Employees) LOOP
        v_total_presents := 0;
        v_total_absents := 0;

        FOR rec IN (
            SELECT status FROM Attendance 
            WHERE employee_id = emp.employee_id
              AND EXTRACT(MONTH FROM attendance_date) = p_month
              AND EXTRACT(YEAR FROM attendance_date) = p_year
        ) LOOP
            IF rec.status = 'Present' THEN
                v_total_presents := v_total_presents + 1;
            ELSIF rec.status = 'Absent' THEN
                v_total_absents := v_total_absents + 1;
            END IF;
        END LOOP;

        IF v_total_presents = 0 AND v_total_absents = 0 THEN
            DBMS_OUTPUT.PUT_LINE('No attendance records found for ' || emp.first_name || ' ' || emp.last_name || ' in the specified month.');
        ELSE
            v_attendance_percentage := (v_total_presents / v_days_in_month) * 100;
            
            DBMS_OUTPUT.PUT_LINE('Employee: ' || emp.first_name || ' ' || emp.last_name);
            DBMS_OUTPUT.PUT_LINE('Total Presents: ' || v_total_presents);
            DBMS_OUTPUT.PUT_LINE('Total Absents: ' || v_total_absents);
            DBMS_OUTPUT.PUT_LINE('Attendance Percentage: ' || ROUND(v_attendance_percentage, 2) || '%');
        END IF;
    END LOOP;

EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('No data found for the specified month and year.');
END Calculate_Attendance_Stats;
/
```

# Output:

![WhatsApp Image 2024-11-01 at 19 13 10](https://github.com/user-attachments/assets/9c3f2460-8c3b-429b-ba5a-df85a570478a)

![WhatsApp Image 2024-11-01 at 19 13 10 (1)](https://github.com/user-attachments/assets/a758158d-515f-4f09-baba-8c6d09d9b421)

# Closing remarks:
## _for more explanation; please proceed to the staging branch of the repository._

## THANKS FOR YOUR TIME!
