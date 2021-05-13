## API与数据库调用关系

| 模块名 | AppointmentDoctor                                 |                                                              |
| ------ | ------------------------------------------------- | ------------------------------------------------------------ |
| GET    | /api/appointment/doctor/appointList/{doctorPhone} | user_info, healthguide_appointment_doctor                    |
| GET    | /api/appointment/doctor/{patientPhone}            | user_info, healthguide_healthrecord_case, healthguide_appointment_doctor |
| GET    | /api/appointment/doctor/department/{patientPhone} | user_info, healthguide_healthrecord_case, healthguide_appointment_doctor |



| 模块名 | AppointmentPatient                              |                                                              |
| ------ | ----------------------------------------------- | ------------------------------------------------------------ |
| GET    | /api/appointment/patient/hospital               | healthguide_appointment_hospital                             |
| GET    | /api/appointment/patient/department/{hospital}  | healthguide_appointment_hospital                             |
| GET    | /api/appointment/patient/doctorList             | healthguide_appointment_doctor                               |
| POST   | /api/appointment/patient/appoint                | healthguide_appointment_doctor, healthguide_appointment_patient_appoint |
| GET    | /api/appointment/patient/appoint/{patientPhone} | healthguide_appointment_patient_appoint                      |
| GET    | /api/appointment/patient/appoint/doctor         | healthguide_appointment_patient_appoint                      |
| DELETE | /api/appointment/patient/withdraw               | healthguide_appointment_patient_appoint                      |



| 模块名 | Auth                       |           |
| ------ | -------------------------- | --------- |
| POST   | /api/login                 | user_info |
| POST   | /api/join                  | user_info |
| GET    | /api/user/info/{userPhone} | user_info |
| GET    | /api/user/info             | user_info |



| 模块名 | ExamCovid                               |                                                              |
| ------ | --------------------------------------- | ------------------------------------------------------------ |
| GET    | /api/exam/covid/hospital                | healthguide_exam_covid_capacity                              |
| GET    | /api/exam/covid/remainder               | healthguide_exam_covid_remainder                             |
| GET    | /api/exam/covid/appointment/{userPhone} | healthguide_exam_covid_appointment                           |
| POST   | /api/exam/covid/appointment             | healthguide_exam_covid_remainder, healthguide_exam_covid_appointment, healthguide_exam_covid_remainder |
| GET    | /api/exam/covid/setting                 | healthguide_exam_covid_capacity, healthguide_exam_covid_remainder |
| PUT    | /api/exam/covid/setting                 | healthguide_exam_covid_capacity, healthguide_exam_covid_remainder |
| GET    | /api/exam/covid/report/{appointId}      | healthguide_exam_covid_report                                |



| 模块名 | ExamPhysical                               |                                                              |
| ------ | ------------------------------------------ | ------------------------------------------------------------ |
| GET    | /api/exam/physical/hospital                | healthguide_exam_physical_capacity                           |
| GET    | /api/exam/physical/remainder               | healthguide_exam_physical_remainder                          |
| GET    | /api/exam/physical/appointment/{userPhone} | healthguide_exam_physical_appointment                        |
| POST   | /api/exam/physical/appointment             | healthguide_exam_physical_remainder, healthguide_exam_physical_appointment |
| GET    | /api/exam/physical/setting                 | healthguide_exam_physical_capacity, healthguide_exam_physical_remainder |
| PUT    | /api/exam/physical/setting                 | healthguide_exam_physical_capacity, healthguide_exam_physical_remainder |
| GET    | /api/exam/physical/report/{appointId}      | healthguide_exam_physical_report                             |



| 模块名 | ForumPost            |                              |
| ------ | -------------------- | ---------------------------- |
| GET    | /api/forum/post      | healthguide_forum_post_topic |
| POST   | /api/forum/post      | healthguide_forum_post_topic |
| GET    | /api/forum/post/{id} | healthguide_forum_post_topic |
| PUT    | /api/forum/post/{id} | healthguide_forum_post_topic |
| DELETE | /api/forum/post/{id} | healthguide_forum_post_topic |



| 模块名 | HealthrecordCase                   |                                          |
| ------ | ---------------------------------- | ---------------------------------------- |
| GET    | /api/healthrecord/case/{userPhone} | user_info, healthguide_healthrecord_case |
| GET    | /api/healthrecord/case/detail      | healthguide_healthrecord_case            |



| 模块名 | HealthrecordPersonInfo                   |           |
| ------ | ---------------------------------------- | --------- |
| GET    | /api/healthrecord/personInfo/{userPhone} | user_info |



| 模块名 | PharBooth                   |                                 |
| ------ | --------------------------- | ------------------------------- |
| GET    | /api/phar/booth/detail/{id} | healthguide_phar_booth_medicine |
| GET    | /api/phar/booth/search      | healthguide_phar_booth_medicine |
| GET    | /api/phar/booth/list        | healthguide_phar_booth_medicine |

