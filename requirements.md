# Requirements — Clinic Management System

## 1. Purpose and scope

A command-line application for a small clinic. Staff keep a register
of patients, book appointments with doctors, and the system prevents
overlapping bookings. A doctor can view the schedule, book follow-up
visits, and keep medical notes on a patient.

The program is designed for one user at a terminal and works without
a network. Payments, reporting, and data exchange with external
systems are out of scope.

## 2. Operating modes

The system runs in one of three modes:

| Mode | Used by | Purpose |
|---|---|---|
| Reception | Front-desk staff | Registering patients, booking, rescheduling and cancelling appointments, searching the register |
| Doctor | Clinic doctor | Viewing the schedule, booking visits, keeping medical notes |
| Patient | Patient — via a self-service kiosk in the lobby | Viewing own appointments, self-booking, cancelling and rescheduling own visits |

No authentication is performed.

The Reception and Doctor modes are selected at startup and can be
switched from the main menu at any time. They share the same set of
appointment operations; Doctor mode differs by pre-filling fields
automatically and by giving access to medical notes.

Patient mode is started separately (`python3 main.py --mode patient`)
and does not allow switching to the staff menus. A patient works only
with their own appointments and sees neither other patients nor the
contents of other patients' visits.

## 3. Functional requirements

### 3.1 Modes

**FR-1.** As a staff member, I want to choose Reception or Doctor
mode at startup so that I see the menu that matches my job.

**FR-2.** As a staff member, I want to switch between Reception and
Doctor mode from the main menu so that I do not have to restart the
program.

**FR-2a.** As a clinic administrator, I want to start the program
directly in Patient mode (command-line argument `--mode patient`)
so that the self-service kiosk in the lobby cannot be switched to
the staff menus.

**FR-2b.** As a patient, I want to end my session and return the
kiosk to the patient-selection screen so that the next person cannot
see my appointments. Exiting from Patient mode to the mode-selection
menu is not provided.

**FR-3.** As a doctor, I want to select myself from the list of
doctors when entering Doctor mode so that the system knows whose
schedule to show and under whose name to save notes.

### 3.2 Patients

**FR-4.** As a receptionist, I want to register a new patient so
that their details are in the system before their first visit.
First name, last name, age and phone are required; address, email
and access needs are optional and skipped with empty input.

**FR-4a.** As a receptionist, I want to record the practical
requirements for a patient's visit — step-free access, an
interpreter, an accompanying person, extra appointment time — so
that we can prepare in advance.

**FR-5.** As a receptionist, I want to find a patient by part of
their name or by ID so that I can find them quickly while they are
at the desk.

**FR-6.** As a receptionist, I want to find a patient by typing the
name without Norwegian letters (`Odegard` for `Ødegård`) and without
matching the case, so that I do not have to guess the exact spelling.

**FR-7.** As a receptionist, I want to see a clear "patient not
found" message when a search returns nothing, so that I know the
patient is not registered rather than assuming the program failed.

**FR-8.** As a receptionist, I want to view the list of all
registered patients so that I can check the register when I am
unsure of a spelling.

### 3.3 Doctors

**FR-9.** As a user, I want to view the list of doctors with their
specialties so that I can direct a patient to the right specialist.

**FR-10.** As a user, I want to view a doctor's working schedule by
weekday so that I know on which days the doctor can be offered to
a patient at all.

Adding doctors through the menu is not provided (see Section 7).

### 3.4 Appointments

**FR-11.** As a user, I want to book a patient with a chosen doctor
for a chosen date, time and duration (30 or 60 minutes) so that the
visit is reserved.

**FR-12.** As a user, I want a booking that overlaps an existing
appointment of that doctor to be rejected, with the conflicting
appointment shown, so that I do not create a double booking.

**FR-13.** As a user, I want a booking on a day when the doctor does
not work to be rejected, so that a visit is not scheduled for an
empty office.

**FR-14.** As a user, I want a booking outside the doctor's working
hours to be rejected, so that a patient is not given an impossible
time.

**FR-15.** As a user, I want a booking to be rejected when the
patient already has a visit with another doctor at that time, so
that the patient is not booked for two appointments at once.

**FR-16.** As a user, I want every rejection to show the doctor's
nearest free slots that also suit the patient, so that I can offer
another time immediately instead of starting the search over.

**FR-17.** As a user, I want to pick one of the suggested slots and
book the patient directly from that list, so that I do not have to
enter the date and time again.

**FR-18.** As a user, I want to cancel an upcoming appointment so
that the time becomes available to other patients.

**FR-19.** As a user, I want to reschedule an upcoming appointment
to another date or time, with the same checks as when booking, so
that I do not have to cancel it and re-enter everything.

**FR-20.** As a user, I want to view all appointments of one
patient, including past and cancelled ones, so that I can answer
"when am I due back?".

**FR-21.** As a user, I want to add or edit a service note on an
appointment — e.g. "interpreter needed" or "asked to be called
back" — so that organisational information reaches a colleague.

**FR-22.** As a doctor, I want to create a new appointment directly
from my schedule without entering the patient again, so that I can
book a follow-up visit at the end of a consultation. The patient is
taken from the current appointment; the doctor defaults to me but
can be changed to a colleague.

### 3.5 Schedule

**FR-23.** As a user, I want to view a chosen doctor's schedule for
a chosen day, ordered by time, so that I can see who is being seen
and when. In Doctor mode, the doctor's own name is offered first.

**FR-24.** As a user, I want to see a doctor's free slots on a
chosen day so that I know when an extra patient can be taken.

### 3.6 Medical notes

**FR-25.** As a doctor, I want to view a patient's medical notes in
chronological order with author and date, so that I can see the
history of observations before a consultation.

**FR-26.** As a doctor, I want to add a medical note to a patient
under my own name, so that the outcome of a consultation is
recorded.

**FR-27.** As a receptionist, I want my menu to contain only
organisational functions so that I am not distracted by clinical
information. The medical-notes section is not available in
Reception mode.

### 3.7 Patient self-service

**FR-28.** As a patient, I want to select myself when entering
Patient mode so that the system shows my appointments.

**FR-29.** As a patient, I want to see the list of my upcoming and
past visits with the doctor, date, time and room, so that I do not
have to call the reception for this information.

**FR-30.** As a patient, I want to choose a specialty and see the
free slots of doctors with that specialty over the coming days, so
that I can book on my own without taking up staff time.

**FR-31.** As a patient, I want to see only a doctor's free time,
without the names or visit reasons of other patients, so that
information about other people stays inaccessible.

**FR-32.** As a patient, I want to state a short reason for the
visit when booking, so that the doctor knows in advance what I am
coming with.

**FR-33.** As a patient, I want to cancel or reschedule my upcoming
visit so that I do not have to come to the clinic just to change
a time.

**FR-34.** As a patient, I want a clear rejection when I try to book
more than the allowed number of upcoming visits, so that I
understand I must cancel an existing appointment first.

**FR-35.** As a patient, I want to book appointments with several
doctors and on several dates, so that I can plan a full
examination. Each appointment is made separately, by invoking the
menu item again.

### 3.8 Data between runs

**FR-36.** As a user, I want patients, doctors, appointments and
notes to survive program restarts, so that no data is lost when
the terminal is closed.

## 4. Non-functional requirements

**NFR-1. Input robustness.** Any invalid input — letters where a
number or date is expected, an unknown ID, a date in the past, an
empty string — leads to a clear message and a chance to retry,
never to a crash.

**NFR-2. Data robustness.** A corrupted or inaccessible database
file leads to a clear message naming the problem, not to a stack
trace.

**NFR-3. Cross-platform.** The program runs on macOS, Linux and
Windows. Paths are built with `pathlib`, files are opened with
explicit UTF-8 encoding, no platform-specific commands are invoked.
Colour output (NFR-9) uses ANSI escape sequences and is disabled
automatically where the terminal does not support them.

**NFR-4. Runs without installing dependencies.** The main program
uses only the Python standard library (including the built-in
`sqlite3` and `unittest`) and starts with `python3 main.py`, with
no virtual environment and no package installation. External
libraries are allowed only in auxiliary scripts (see NFR-5).

**NFR-5. Auxiliary scripts.** Extra tools outside the main
workflow — for example a doctor-workload chart built with
`matplotlib` — live separately from the program, have their own
dependency file, and are run by a separate command. The main
program works whether or not their packages are installed.

**NFR-6. Reproducible setup.** `README.md` states the required
Python version (3.14 or newer), the run command for macOS, Linux
and Windows, the location of the database file, and how to reset
the demonstration data.

**NFR-7. Understandable output.** Error messages explain the cause
and suggest the next step. The user never sees internal terms or
stack traces.

**NFR-8. Readable code.** Object-oriented structure, separation
into modules, a docstring on every class and function.

**NFR-9. Visual mode distinction.** Each mode has its own colour:
Reception — blue, Doctor — green, Patient — yellow. The current
mode's name is always printed as text in the screen header. If the
terminal does not support colour, or the `NO_COLOR` environment
variable is set, output contains no escape sequences.

**NFR-10. Testability.** Conflict checking and free-slot search are
implemented in Python as functions over plain objects, with no SQL
inside, and are covered by automated tests that do not touch the
database.

## 5. Data model

### Patient

| Field | Type | Notes |
|---|---|---|
| patient_id | int | Assigned by the database (autoincrement) |
| first_name | str | Required, non-empty |
| last_name | str | Required, non-empty |
| age | int | 0–120 |
| phone | str | Required |
| email | str | Optional |
| street | str | Street and house number. Optional |
| postal_code | str | Postal code. Optional |
| city | str | Town or city. Optional |
| access_needs | str | Practical visit requirements. Optional |

The `access_needs` field holds practical requirements for a visit:
step-free access, an accompanying person, an interpreter, extra
appointment time. Diagnoses and health information are never
entered here.

### Doctor

| Field | Type | Notes |
|---|---|---|
| doctor_id | int | Assigned by the database (autoincrement) |
| first_name | str | Required, non-empty |
| last_name | str | Required, non-empty |
| specialty | str | E.g. "Fastlege", "Hudlege" |
| phone | str | Internal number. Optional |
| email | str | Work email. Optional |
| room | str | Office. Optional |

### Doctor working day

A separate entity: one doctor has several working days.

| Field | Type | Notes |
|---|---|---|
| doctor_id | int | Reference to the doctor |
| weekday | int | 0 — Monday … 6 — Sunday |
| start_time | time | Start of the working day |
| end_time | time | End of the working day |

A missing row for a weekday means the doctor does not work that day.

### Appointment

| Field | Type | Notes |
|---|---|---|
| appointment_id | int | Assigned by the database (autoincrement) |
| patient_id | int | Reference to an existing patient |
| doctor_id | int | Reference to an existing doctor |
| start_time | datetime | Date and start time |
| duration_minutes | int | 30 or 60 |
| status | str | "scheduled" or "cancelled" |
| note | str | Service note, editable. Not medical information |

The end time is not stored — it is derived as start plus duration.
Whether a visit took place is not stored either: an appointment
counts as past when its time is before the current moment and it is
not cancelled.

### Medical note

| Field | Type | Notes |
|---|---|---|
| note_id | int | Assigned by the database (autoincrement) |
| patient_id | int | Who it concerns |
| doctor_id | int | Author |
| created_at | datetime | Creation time |
| text | str | Contents |

Attached to the patient, not to a single visit. Available only in
Doctor mode.

### Relationships

| Relationship | Kind | Stored as |
|---|---|---|
| Patient → appointments | one to many | `patient_id` on the appointment |
| Doctor → appointments | one to many | `doctor_id` on the appointment |
| Doctor → working days | one to many | `doctor_id` on the working day |
| Patient → medical notes | one to many | `patient_id` on the note |
| Doctor → medical notes (as author) | one to many | `doctor_id` on the note |

A relationship is stored only on the "many" side. A patient does
not hold a list of their appointments and notes — these are fetched
from the store by `patient_id`.

### Booking result

The structure returned when creating or rescheduling an appointment.

| Field | Type | Notes |
|---|---|---|
| success | bool | Whether the operation succeeded |
| appointment | Appointment | The created appointment, or empty |
| reason | str | Rejection reason: day off, outside hours, doctor busy, patient busy, time in the past |
| alternatives | list | Nearest free slots (up to 5) |

## 6. Business rules

**BR-1.** The clinic works on a half-hour grid: an appointment
starts on the hour or half-hour and lasts 30 or 60 minutes.

**BR-2.** An appointment fits entirely within the doctor's working
hours for that weekday.

**BR-3.** Two active appointments of the same doctor never overlap.
Overlap is defined as `start_A < end_B and start_B < end_A`.

**BR-4.** A cancelled appointment does not occupy time but stays in
the system with status "cancelled".

**BR-5.** One patient cannot have two overlapping appointments,
even with different doctors. Several visits back to back or on
different days are allowed.

**BR-6.** Booking in the past is impossible. Only upcoming
appointments can be cancelled or rescheduled.

**BR-7.** Identifiers are assigned by the database; the user never
enters them.

**BR-8.** On rejection, the system suggests up to five of the
doctor's nearest free slots. Only slots free for both the doctor
and the patient are suggested. If the requested day has no suitable
slots, slots from the doctor's next working day are suggested.

**BR-9.** Rescheduling passes the same checks as booking (BR-1,
BR-2, BR-3, BR-5, BR-6). The appointment being moved is excluded
from the conflict check.

**BR-10.** Patient search matches a substring of the first or last
name, case-insensitively and diacritics-insensitively: `ø` and `o`,
`å` and `a`, `æ` and `ae` are treated as equal. Only the original
spelling is stored and displayed.

**BR-11.** A medical note is never edited or deleted — only a new
one is added.

**BR-12.** The service note on an appointment (`note`) is editable
in Reception and Doctor modes and is not meant for clinical
information. A patient fills this field once, when creating the
appointment (reason for the visit); after creation the field is not
shown in Patient mode.

**BR-13.** Patient mode gives access only to the selected patient's
appointments. A doctor's schedule is shown as a set of free slots
with no information about occupied visits: no names, no reasons,
no indication of who holds the time.

**BR-14.** A patient cannot have more than ten upcoming
appointments at once. Cancelled and past visits do not count.
The limit applies only to self-booking; reception staff and doctors
may exceed it.

**BR-15.** A patient may cancel or reschedule their visit no later
than two hours before it starts. After that, changes are possible
only through the reception. The boundary is defined as
`start_time − current moment ≥ 2 hours`.

**BR-16.** Every rule that depends on the current moment (BR-6,
BR-15 and the past-visit test) compares the appointment time with
a moment passed into the check from outside, not read inside it.

**BR-17.** The `access_needs` field is available in Reception and
Doctor modes and contains no health information.

## 7. Storage and initial data

All data — patients, doctors, schedules, appointments and notes —
is stored in an SQLite database file (`data/clinic.db`) and
survives program restarts.

At startup the system creates the database file and tables if they
do not exist. If the database has just been created, it is seeded
once with demonstration data:

- several doctors with different specialties and different weekly
  schedules;
- several patients, including names with Norwegian letters to
  exercise the search;
- several appointments, some of which fill one doctor's day
  completely;
- several medical notes on one of the patients.

The seed data is defined in code, in a separate module. The dates
of the demonstration appointments are given as offsets from the
current date ("tomorrow", "in two days"), not as absolute values.

Seeding runs only when a new database is created. To reset the
system to its initial state, delete `data/clinic.db` and start the
program again; this is described in the README.

## 8. Assumptions

- One clinic, one time zone.
- Public holidays and vacations are not taken into account.
- A doctor's schedule is the same from week to week.
- The set of doctors and their schedules do not change through the
  program after the database is created.
- Mode separation is not a security mechanism. All data in the
  system is demonstration data.
- The primary scenario is a single running instance. Data integrity
  does not depend on this assumption: every check runs at operation
  time (see Section 9), so a stale screen leads to an ordinary
  rejection, not a double booking.
- Patient mode is separated from the staff modes by the way the
  program is started. The lobby kiosk is started by an
  administrator with the `--mode patient` flag.
- Patient identification is done by choosing from a list.
- Each doctor has one fixed room; room occupancy is not tracked.
- No country field in the address.

## 9. Design constraints

- The `Patient`, `Doctor`, `Appointment` and `MedicalNote` classes
  hold data only and contain no database code.
- All SQL is concentrated in a single data-access module.
- Conflict checking and free-slot search take and return plain
  Python objects; there is no database access inside these
  functions.
- Every check (BR-1…BR-6, BR-14, BR-15) runs at operation time
  against the current database state, not at menu-display time.
  The free-slot list on screen is a hint; the check at booking
  time decides.
- The current moment is obtained once at the top level of the
  program and passed into the business logic as a parameter. There
  are no `datetime.now()` calls inside the checking and slot-search
  functions.
- Time is stored and compared without time-zone information.
- Colour escape sequences are concentrated in a single output
  module. Colour support is detected once at startup; the business
  logic knows nothing about colour.
- Clinical information lives in a separate entity and is not a
  field of the appointment.

## 10. Out of scope for this version

Electronic patient identification and real access control, a web
interface for self-booking, appointment reminders by SMS or email,
a graphical interface, editing the doctor register from within the
program, test results and structured medical data, printed
confirmations, in-program statistics and reporting, suggesting an
alternative doctor of the same specialty, public-holiday handling.

Doctor vacations and stand-in (substitute) doctors were considered
and deliberately deferred.