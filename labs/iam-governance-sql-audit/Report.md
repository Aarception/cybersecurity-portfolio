# Automating Identity and Access Management (IAM) Compliance via SQL



### Project description

This report documents SQL queries executed to support authentication oversight, geographic access validation, and department‑specific update governance. Each query was initiated in response to a distinct operational need and used logical operators (`AND`, `OR`, `NOT`) and pattern matching (`LIKE`) to isolate relevant records from authentication logs and employee datasets. The resulting outputs informed access governance decisions, risk assessments, and compliance workflows across multiple business units.

---

## 1\. Evaluating after‑hours access governance

**Trigger:**  
Unusual login failures surfaced in the monitoring dashboard outside approved operating hours, prompting a review of after‑hours authentication behavior for potential policy deviations.

**Query**

```sql
SELECT \*
FROM log\_in\_attempts
WHERE login\_time > '18:00'
  AND success = 0;
```

**Operational purpose**  
This filter isolates failed authentication attempts occurring after business hours. The combination of `login\_time > '18:00'` and `success = 0` narrows the dataset to events that represent elevated risk: unsuccessful access attempts outside normal operating windows.

!\[Screenshot](screenshots/1.png)



## 2\. Reviewing activity surrounding a flagged date

**Trigger:**  
A login event on 2022‑05‑09 displayed irregular characteristics. To determine whether it was isolated or part of a broader pattern, the review expanded to include the previous day.

**Query**

```sql
SELECT \*
FROM log\_in\_attempts
WHERE login\_date = '2022-05-09'
   OR login\_date = '2022-05-08';
```

**Operational purpose**  
This filter retrieves all authentication activity surrounding the flagged date. Using `OR` expands the scope to include both the incident date and the preceding day, enabling a complete review of the 48‑hour window for related activity or precursor events.

!\[Screenshot](screenshots/2.png)

---

## 3\. Enforcing geographic access controls

**Trigger:**  
Geolocation analysis ruled out Mexico as a relevant source of interest. To maintain alignment with geographic access policies, the dataset needed to exclude all Mexico‑based entries.

**Query**

```sql
SELECT \*
FROM log\_in\_attempts
WHERE NOT country LIKE 'MEX%';
```

**Operational purpose**  
This filter excludes all login attempts originating from Mexico. The `LIKE 'MEX%'` pattern captures both `MEX` and `MEXICO`, and applying `NOT` removes those entries, producing a dataset focused solely on foreign authentication sources relevant to the investigation.

!\[Screenshot](screenshots/3.png)

---

## 4\. Scoping Marketing assets for update compliance

**Trigger:**  
A maintenance ticket identified outdated software versions on systems assigned to Marketing personnel in the East building. A targeted list was required to ensure update compliance.

**Query**

```sql
SELECT \*
FROM employees
WHERE department LIKE 'Marketing'
  AND office LIKE 'East-%';
```

**Operational purpose**  
This filter identifies Marketing personnel assigned to East‑building offices. The combination of `department LIKE '%Marketing%'` and `office LIKE 'East-%'` ensures the result set includes only the machines within the targeted department and physical location for the required update.

!\[Screenshot](screenshots/4.png)

---

## 5\. Identifying high‑exposure departments for risk mitigation

**Trigger:**  
A phishing campaign targeted Finance and Sales inboxes. The remediation plan required isolating all employees in these departments to validate system integrity.

**Query**

```sql
SELECT \*
FROM employees
WHERE department LIKE 'Finance'
   OR department LIKE 'Sales';
```

**Operational purpose**  
This filter isolates employees in either Finance or Sales. Using `OR` consolidates both departments into a single query, enabling efficient scoping for the update cycle assigned to these groups.

!\[Screenshot](screenshots/5.png)

---

## 6\. Validating patch compliance across non‑IT departments

**Trigger:**  
The IT department completed its updates ahead of schedule. The remaining task was to identify all other employees whose systems still required patching to maintain compliance coverage.

**Query**

```sql
SELECT \*
FROM employees
WHERE NOT department LIKE 'Information Technology';
```

**Operational purpose**  
This filter excludes the Information Technology department, which has already completed the required update. Applying `NOT` ensures the result set contains only employees whose systems remain pending, allowing the team to focus on outstanding assets.

!\[Screenshot](screenshots/6.png)

---

## Summary

These SQL queries supported authentication oversight, geographic access validation, and department‑specific compliance tasks. Logical operators (`AND`, `OR`, `NOT`) and pattern matching (`LIKE`) were used to isolate failed after‑hours logins, analyze activity around a flagged date, exclude irrelevant geographic sources, and identify employees requiring targeted updates. The resulting datasets informed access governance decisions, risk assessments, and maintenance workflows across multiple business units.

---

## Analyst Impact

The SQL queries translated raw authentication and employee data into targeted oversight actions. Each query supported a specific control objective by tightening visibility into access behavior, geographic patterns, and update compliance.

* **Reinforced access control** by isolating failed after‑hours logins for rapid deviation detection.
* **Improved incident clarity** through expanded date‑range analysis around the flagged event.
* **Focused geographic review** by excluding non‑relevant Mexico‑based activity.
* **Streamlined update compliance** by pinpointing outdated Marketing assets in the East building.
* **Accelerated phishing response** by consolidating Finance and Sales personnel for integrity checks.
* **Maintained patch coverage** by filtering out already‑updated IT systems to prioritize remaining assets.

**Overall, these SQL‑driven insights strengthened identity and access oversight by enabling precise scoping, faster triage, and more efficient risk mitigation.**

