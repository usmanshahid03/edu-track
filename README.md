A relational database system that manages student and teacher records, course enrollments, attendance, and academic performance for educational institutions — all in one structured platform.

📋 Table of Contents

Overview
Database Schema
Tables
Relationships
Setup & Installation
Usage


Overview
EduTrack (FSMS) is built on a MariaDB/MySQL relational database. It organizes institutions into departments and sections, tracks student and teacher profiles, manages course enrollments, logs attendance, and records academic marks and grades.

Database Schema
Database Name: fsms
Server: MariaDB 10.4.24
Character Set: utf8mb4

Tables
TableDescriptionstudentStudent personal details, department, and sectionteacherTeacher profiles and contact informationdepartmentDepartments with assigned Head of Department (HOD)sectionSections within departments with enrollment countcourseCourses with credit hoursenrollmentLinks students to courses, sections, and teachersattendanceTracks student attendance per datemarksRecords obtained and total marks per assessment topicgradingStores final grading marks per enrollmenttaughtMaps teachers to courses and sections

Relationships

A Department has many Sections and is headed by a Teacher (HOD)
A Student belongs to one Department and one Section
An Enrollment links a Student to a Course, Section, and Teacher
Attendance, Marks, and Grading are all tied to an Enrollment
A Teacher can teach multiple Courses across different Sections


Setup & Installation

Make sure you have MySQL or MariaDB installed.
Open your database client (e.g., phpMyAdmin or MySQL CLI).
Create the database:

sql   CREATE DATABASE fsms;

Import the SQL dump:

bash   mysql -u root -p fsms < fsms.sql

Usage
Once imported, you can query the database to:

View all students in a department or section
Check a student's enrolled courses and assigned teacher
Retrieve attendance records by date or student
Calculate total marks obtained per student per course
List courses taught by a specific teacher

Example — Get all students with their enrolled courses:
sqlSELECT s.fname, s.lname, c.courseName, t.fname AS teacher
FROM student s
JOIN enrollment e ON s.stdID = e.stdID
JOIN course c ON e.courseID = c.courseID
JOIN teacher t ON e.teacherID = t.teacherID;

Stored Procedures & Triggers

insert_student — Stored procedure to insert a new student record.
update_enrollment_count — Trigger that automatically updates the enrollment count in the section table after each new enrollment.


Notes

Passwords are currently stored as plain text. It is strongly recommended to hash passwords before use in production.
Some date fields use varchar instead of DATE type — consider standardizing date formats for consistency.
The attendance table lacks a primary key; adding one is recommended for data integrity.
