--Student Template 
 
SELECT
CASE 
WHEN st.schoolID = '9' THEN '05615640530055' -- Sierra Hills Education Center 
WHEN st.schoolID = '10' THEN '05615640531509' -- Calaveras High
WHEN st.schoolID = '11' THEN '05615640530022' -- Gold Strike High
END as 'PrimaryCDSCode'
,st.studentNumber as 'LocalStudentID'
,st.stateID as 'StateID'
,st.firstName as 'FirstName'
,Case 
  WHEN st.middleName is null then ' ' 
  ELSE st.middleName
END as 'MiddleName'
, st.lastName as 'LastName'
, convert (varchar(10), st.birthdate, 101) as 'DateOfBirth'
, st.gender as 'Gender'
, st.grade as 'GradeLevel'
, CAST(gpa.cum_gpa as DECIMAL(5,2)) AS 'GPA'
, 'weighted' as 'GPAType'
, '' as 'NSLP'
, '' as 'LoteCertSource'
, '' as 'LanguageCode'
, '' as 'FosterYouth'
,
CASE
WHEN p.personID IN (
select personid
from ProgramParticipation
WHERE participationID = 134
) THEN 'N'
ELSE 'Y'
END as 'ParentConsent'
, st.hispanicethnicity as 'HispanicEthnicity'
, st.raceEthnicity as 'RaceCode1'
, '' as 'RaceCode2'
, '' as 'RaceCode3'
, '' as 'RaceCode4'
, '' as 'RaceCode5'
,convert (varchar(10), e.startDate, 101) as 'EnrollmentStartDate'
,CASE 
  when e.endDate is not null then convert (varchar(10), e.endDate, 101) 
  Else '' 
END as 'EnrollmentEndDate'   
 
from student st
  inner join person p on p.personID=st.personid
  inner join [Identity] i on p.currentIdentityID =i.identityID
  inner join enrollment e on e.personid =st.personID and e.calendarID=st.calendarID AND E.ENDYEAR = '2024'
  inner join Calendar c on c.calendarID=e.calendarID
  inner join school s on s.schoolID=c.schoolID
  LEFT join view_gpa_cum gpa on gpa.calendarid = st.calendarid AND gpa.personID=e.personID
 WHERE e.endYear ='2024' 
        and e.grade in ('09','10','11','12') --UPDATE TO INCLUDE 06,07,08 BASED on your MOU
        and c.schoolID IN ('9','10','11')
       -- AND gpa.calendarid IN ('520','526','516')
       AND e.servicetype = 'P'
       AND e.enddate IS NULL
