# ServiceNow Script-Controlled ACL – Record Access Control

## Project Overview

This project demonstrates how to implement script-controlled Access Control Lists (ACLs) in ServiceNow to restrict record-level access based on user roles and the value of a record field.

In this lab, the custom `u_institution_details` table contains student/institution records with different branch values. The READ ACL is configured so that EEE branch records are accessible to users with the required role, while administrators retain full access. Additional CREATE, WRITE, and DELETE ACLs are configured using separate custom roles.

## Objectives

- Create a test ServiceNow user and custom roles.
- Create the `u_institution_details` custom table.
- Add student and institution-related fields.
- Configure script-controlled READ, CREATE, WRITE, and DELETE ACLs.
- Use roles `bb1`, `bb2`, `bb3`, and `bb4` for different access operations.
- Verify access by impersonating users with different roles.

## Roles Used

| Role | ACL Operation | Purpose |
|---|---|---|
| `bb1` | Read | View the permitted EEE records |
| `bb2` | Create | Create records |
| `bb3` | Write | Edit records |
| `bb4` | Delete | Delete records |
| `admin` | Full access | Administrative access |

## Custom Table

**Label:** Institution Details  
**Name:** `u_institution_details`

### Fields

- Student Roll Number – Auto Number
- Student Name – Reference: User
- Faculty Name – Reference: User
- Branch – Choice: ECE, EEE, CSE
- Email – String
- Phone Number – String
- Description – Multi String

## READ ACL

The READ ACL is configured as a record-level ACL for `u_institution_details` with the operation set to `read` and Advanced enabled.

The documented configuration uses the `bb1` role and a Branch = EEE data condition. The script allows administrators and users with the `bb1` role and denies other users.

```javascript
(function () {
    // Allow admin users full access
    if (gs.hasRole('admin')) {
        return true;
    }

    // Allow users with the bb1 role to view EEE branch records
    if (gs.hasRole('bb1')) {
        return true;
    }

    // Deny access for all others
    return false;
})();
```

## CREATE ACL

- Type: Record
- Operation: Create
- Name: `u_institution_details`
- Active: True
- Required role: `bb2`
- No data condition is used.

## WRITE ACL

- Type: Record
- Operation: Write
- Name: `u_institution_details`
- Active: True
- Required role: `bb3`
- No data condition is used.

## DELETE ACL

- Type: Record
- Operation: Delete
- Name: `u_institution_details`
- Active: True
- Required role: `bb4`
- No data condition is used.

## Verification

The project verifies the ACL configuration by impersonating users and opening the `u_institution_details` list.

Expected verification from the lab:

- A user with `bb1` can view the EEE branch records.
- A user without the required role cannot view records.
- An administrator can view all records regardless of branch.
- Users with `bb1`, `bb2`, and `bb3` can view, create, and edit the permitted EEE records.
- Users with `bb1`, `bb2`, `bb3`, and `bb4` can additionally delete the permitted EEE records.

## ServiceNow Concepts Demonstrated

- Access Control Lists (ACLs)
- Record-level security
- Role-based access control
- Script-controlled ACLs
- Data conditions
- User impersonation for testing
- READ / CREATE / WRITE / DELETE security operations

## Project Evidence

The original lab document is included as `ACL.docx`. It contains the configuration steps and screenshots used to demonstrate the implementation and verification.

## Outcome

The lab demonstrates how script-controlled READ, WRITE, CREATE, and DELETE ACLs can enforce record-level security by evaluating user roles together with record field values.

---

**Project:** ServiceNow Script-Controlled ACL  
**Platform:** ServiceNow  
**Focus:** Access Control, Roles, Record-Level Security, ACL Scripting
