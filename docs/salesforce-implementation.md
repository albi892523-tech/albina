# Salesforce Implementation

## Objects

### Student
Stores student details such as name, email, phone, date of birth, address, and student status.

### Course
Stores course name, category, duration, course fee, and description.

### Instructor
Stores instructor name, instructor code, expertise, phone, and email.

### Enrollment
Stores relationships to Student, Course, and Instructor, plus enrollment date, enrollment status, fees paid, total amount, and comments.

## Relationships
Enrollment has lookup relationships to:
- Student
- Course
- Instructor

## Validation Rules

### Student Email Domain
The source project specifies validation to ensure the student uses an allowed institutional email domain.

### Fees Before Approval
The source project specifies preventing approval when fees have not been paid.

Conceptual formula:
```
AND(
    ISPICKVAL(Enrollment_Status__c, "Approved"),
    Fees_Paid__c = FALSE
)
```

## Approval Process
Approval_Enrollment_Request:
- Entry status: Requested
- Approver: Training Manager
- Final approval: set status to Approved
- Final rejection: set status to Rejected
- Rejection notification: email alert to student

## Automation Flows
1. Set enrollment date when an enrollment record is created.
2. When an enrollment is approved, notify the student and set the student status to Active.
3. Auto-assign an instructor according to course category:
   - Technical → Instructor A
   - Language → Instructor B
   - Non-Technical → Instructor C
4. When an enrollment is Requested, create a follow-up task for the administrator.

## Reports
- Students by Status
- Enrollments by Course
- Pending Enrollment Approvals
- Revenue Report (Fees Paid)

## Dashboard
The source project specifies a Student Management Dashboard containing:
- Donut chart: Students by Status
- Bar chart: Enrollments by Course
- Gauge: Revenue Report – Fees Paid
- Table: Pending Enrollment Approvals

## Security
The source project specifies profiles for Admin, Enrollment Officer, and Instructor, plus permission sets and organization-wide defaults. Enrollment is private while Student and Course records are public read/write in the documented setup.
