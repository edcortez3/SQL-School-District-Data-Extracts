/*FROM section se
inner join SectionPlacement sp on sp.sectionID =se.sectionID --and sp.termID ='733'
INNER JOIN term t ON t.termid = sp.termid
ORDER BY startdate DESC */

--with temp As
SELECT 
DISTINCT st.stateID as 'StateID'
, CONCAT(tc.districtnumber, tc.schoolnumber)
        AS 'CDSCode'
, CASE
        WHEN tc.schoolname = 'Sierra Hills Education Center' THEN '050364'
        WHEN tc.schoolname = 'Calaveras High School' THEN '052760' 
        WHEN tc.schoolname = 'Gold Strike High School' THEN '111111' -- confirmed with data lead that Gold Strike High does not have a CEEB Code
        ELSE '111111' 
        END AS 'ATPCode'
, 'N' AS 'IsWorkInProgress'
, tc.grade AS 'GradeLevel'
, concat (tc.startYear, '-'
, right (tc.endyear,2)) as 'Schoolyear'
, CASE 

        WHEN e.personid IN (
        
         SELECT personid
         FROM transcriptcourse 
         WHERE tc.schoolName = 'Sierra Hills Education Center') THEN 
   
        CASE 
    WHEN tc.actualTerm = '1' AND tc.termslong = '1' THEN 'S1'
    WHEN tc.actualTerm = '1' AND tc.termslong = '2' THEN 'S1'
    WHEN tc.actualTerm = '2' AND tc.termslong = '2' THEN 'S2'
   -- WHEN tc.actualTerm = '1' AND tc.termslong = '4' THEN 'Q1'
                     WHEN tc.actualTerm = '2' AND tc.termslong = '4' THEN 'S1'
                    -- WHEN tc.actualTerm = '3' AND tc.termslong = '4' THEN 'Q3'
                     WHEN tc.actualTerm = '4' AND tc.termslong = '4' THEN 'S2'
                     WHEN tc.actualTerm = '5' AND tc.termslong = '' THEN 'SS1'
    WHEN tc.actualTerm = '5' THEN 'SS1'
    ELSE ''
END 
 
 
    
    WHEN tc.actualTerm = '1' AND tc.termslong = '1' THEN 'S1'
    WHEN tc.actualTerm = '1' AND tc.termslong = '2' THEN 'S1'
    WHEN tc.actualTerm = '2' AND tc.termslong = '2' THEN 'S2'
    WHEN tc.actualTerm = '1' AND tc.termslong = '4' THEN 'Q1'
                     WHEN tc.actualTerm = '2' AND tc.termslong = '4' THEN 'Q2'
                     WHEN tc.actualTerm = '3' AND tc.termslong = '4' THEN 'Q3'
                     WHEN tc.actualTerm = '4' AND tc.termslong = '4' THEN 'Q4'
                     WHEN tc.actualTerm = '5' AND tc.termslong = '' THEN 'SS1'
    WHEN tc.actualTerm = '5' THEN 'SS1'
    ELSE ''
    

  END AS 'Term'
 ,
tc.courseNumber as 'LocalCourseId',
replace(tc.courseName,',','') as 'TranscriptAbbreviation',
CASE 
        WHEN collegecode is null then 'Z' 
WHEN collegecode like 'G%' then 'G' 
ELSE collegecode end as 'SubjectArea', 
Case 
WHEN tc.coursename like 'AP %' then 'AP' 
WHEN tc.coursename like '%9H' then 'H'  --District Specific 
WHEN tc.coursename like '%0H' then 'H' --District Specific 
WHEN tc.coursename like '%1H' then 'H' --District Specific 
WHEN tc.coursename like '%2H' then 'H' --District Specific 
WHEN tc.coursename like '% H' then 'H'  when tc.coursename like '%Honors' then 'H'  else '' end as 'AcademicIndicator',
tr.creditsAttempted AS 'CreditsAttempted',
tr.creditsEarned AS 'CreditsEarned',
LEFT(tc.score,2) as 'CourseGrade'

from student st
inner join enrollment e on e.personid =st.personID and e.calendarID=st.calendarID
inner join Calendar c on c.calendarID=e.calendarID
--INNER JOIN SectionPlacement
--sp ON sp.sectionID = s.sectionID
inner join school s on s.schoolID=c.schoolID
inner join transcriptCourse tc on tc.personID=st.personID
inner join transcriptCredit tr on tr.transcriptID=tc.transcriptID
WHERE 
e.endYear ='2024' 
        and e.active = 1
and e.grade IN ('09','10','11','12') --update to include 06, 07, 08 per your MOU
and st.grade IN ('09', '10', '11', '12')
-- and c.schoolID IN ('9', '10', '11')
--and tr.standardID IN (3, 5, 6, 8, 9, 10, 11, 12, 13, 14, 16, 18, 21, 22, 23, 
--49, 54, 57, 67, 141, 142, 145, 146, 147, 148, 149, 150) --update to local setup
and score not IN ('CR','ip','ng','p')
AND tc.grade IN ('09', '10', '11', '12')
--)
/*select distinct StateID, TranscriptAbbreviation,LocalCourseId,
actualTerm, endTerm, Term
from temp 
where actualTerm = 5 and endTerm is null
*/
--group by actualTerm, endTerm, Term
--order by 4 desc
/*select
actualTerm, termslong, Term, COUNT (distinct StateID) as COUNT_students
from temp 
--where actualTerm = 5 and endTerm is null
group by actualTerm, termslong, Term
*/
UNION 

--Course Grade Work In Progress
select 
Distinct  st.stateID as 'StateID'
,concat('0561564',s.number) as 'CDSCode'
, CASE
        WHEN s.name = 'Sierra Hills Education Center' THEN '050364'
        WHEN s.name = 'Calaveras High School' THEN '052760' 
        WHEN s.name = 'Gold Strike High School' THEN '111111' -- confirmed with data lead that Gold Strike High does not have a CEEB Code
        ELSE '111111' 
        END AS 'ATPCode' --update, case statement may be needed. 
, 'Y' as 'IsWorkInProgress'
, e.grade as 'GradeLevel'
, concat (st.startYear, '-', right (st.endyear,2)) as 'Schoolyear'
, CASE 
WHEN t.name =  'Semester 2' THEN 'S2'
WHEN t.name = 'Semester 1' THEN 'S1'
WHEN t.name = 'Y' THEN 'F'
ELSE t.name
END AS  'Term' --review if possible to update to dynamic reference 
,co.Number as 'LocalCourseId',
  replace(co.Name,',','') as 'TranscriptAbbreviation',
case 
when cc.value is null then 'Z' when cc.value like 'G%' then 'G' Else cc.value end as 'SubjectArea', 
Case 
when co.name like 'AP %' then 'AP' 
when co.name like '%9H' then 'H'  when co.name like '%0H' then 'H' --review with District
when co.name like '%1H' then 'H' when co.name like '%2H' then 'H' --review with District
when co.name like '% H' then 'H'  when co.name like '%Honors' then 'H'  --review with District 
else '' end as 'AcademicIndicator',
'0.00' as 'CreditsAttempted','0.00' as 'CreditsEarned',
'WIP' as 'CourseGrade'
from student st
inner join enrollment e on e.personid =st.personID and e.calendarID=st.calendarID
inner join Calendar c on c.calendarID=e.calendarID
inner join school s on s.schoolID=c.schoolID
inner join Roster r on r.personID =st.personID and r.endDate is null 
inner join section se on se.sectionID =r.sectionid
inner join SectionPlacement sp on sp.sectionID =se.sectionID 
        INNER JOIN term t ON t.termid = sp.termid
inner join course co on co.courseID =se.courseID
left outer join CustomCourse cc on cc.courseID =co.courseID and cc.attributeID ='52'
where e.endYear ='2024' 
AND e   .grade IN ('09', '10', '11', '12')
--AND SP.termid IN ('969', '961')
--and co.CALENDARID IN ('526','520','516') --bhs, bjhs,
--may need to restrict on active status and current school of enrollment


	
