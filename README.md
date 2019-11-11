# Query
SELECT AgeRange.ageGroup, Gender.gender, 
       COUNT(person.partyId), ROUND(100 * COUNT(person.partyId) / Total.countOfPeople) AS percentage
FROM (SELECT '00 - 1' AS ageGroup, CURRENT_DATE AS lower, CURRENT_DATE - INTERVAL 1 YEAR AS upper
      UNION ALL
      SELECT '1 - 3', CURRENT_DATE - INTERVAL 1 YEAR, CURRENT_DATE - INTERVAL 3 YEAR 
      UNION ALL
      SELECT '3 - 5', CURRENT_DATE - INTERVAL 1 YEAR, CURRENT_DATE - INTERVAL 3 YEAR 
      UNION ALL
      SELECT '5 - 12', CURRENT_DATE - INTERVAL 1 YEAR, CURRENT_DATE - INTERVAL 3 YEAR 
      UNION ALL
      SELECT '12 - 18', CURRENT_DATE - INTERVAL 1 YEAR, CURRENT_DATE - INTERVAL 3 YEAR 
      UNION ALL
      SELECT '18 - 24', CURRENT_DATE - INTERVAL 18 YEAR, CURRENT_DATE - INTERVAL 25 YEAR
      UNION ALL
      SELECT '25 - 34', CURRENT_DATE - INTERVAL 25 YEAR, CURRENT_DATE - INTERVAL 35 YEAR
      UNION ALL
      SELECT '35 - 44', CURRENT_DATE - INTERVAL 35 YEAR, CURRENT_DATE - INTERVAL 45 YEAR
      UNION ALL
      SELECT '45 - 54', CURRENT_DATE - INTERVAL 45 YEAR, CURRENT_DATE - INTERVAL 55 YEAR
      UNION ALL
      SELECT '55 - 64', CURRENT_DATE - INTERVAL 55 YEAR, CURRENT_DATE - INTERVAL 65 YEAR
      UNION ALL
      SELECT '65+', CURRENT_DATE - INTERVAL 65 YEAR, null
      UNION ALL
      SELECT 'Unknown', null, null)  AgeRange
CROSS JOIN (SELECT 'M' AS Gender
            UNION ALL
            SELECT 'F'
            UNION ALL 
            SELECT 'Unknown') Gender
CROSS JOIN (SELECT COUNT(*) countOfPeople
            FROM person) Total
LEFT JOIN person
       ON ((person.dateOfBirth > AgeRange.upper AND dateOfBirth <= AgeRange.lower)
           OR (person.dateOfBirth <= AgeRange.lower AND AgeRange.upper IS NULL)
           OR (AgeRange.lower IS NULL AND AgeRange.upper IS NULL AND person.dateOfBirth IS NULL))
          AND (Gender.gender = person.gender
               OR Gender.gender = 'Unknown' AND person.gender IS NULL)
GROUP BY AgeRange.ageGroup, Gender.gender



https://blog.edmdesigner.com/sending-email-with-php/
