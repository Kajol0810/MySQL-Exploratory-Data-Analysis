# MySQL-Exploratory-Data-Analysis
-- Exploratory Data Analysis

Performed various operation on the layoffs data
To find the Maximum layoffs
  SELECT MAX(total_laid_off), Max(percentage_laid_off)
  FROM layoffs_staging2;  
  ![image](https://github.com/Kajol0810/MySQL-Exploratory-Data-Analysis/assets/59485729/76de5b50-df96-44c1-9f46-a4e2e64a7f11)

  To find the details of companies who raised the maximum funds
SELECT *
FROM layoffs_staging2
WHERE percentage_laid_off = 1
ORDER BY funds_raised_millions DESC;


To find the company name and number of laid_off by company
SELECT company, SUM(total_laid_off)
FROM layoffs_staging2
GROUP BY company
ORDER BY 2 DESC;   
![image](https://github.com/Kajol0810/MySQL-Exploratory-Data-Analysis/assets/59485729/bf4ee1c9-3e0c-4c40-9d56-47680974e678)
 

SELECT *
FROM layoffs_staging2;

Start and end date of the layoffs data
SELECT MIN(`Date`),  MAX(`Date`)
FROM layoffs_staging2; 
 ![image](https://github.com/Kajol0810/MySQL-Exploratory-Data-Analysis/assets/59485729/1a9dbd9f-6ccc-4094-90a4-5d08879775e9)

Finding the top 5 companies having large number of layoffs
WITH Company_Year (company, years, total_laid_off) AS
(
SELECT company,YEAR(`date`), SUM(total_laid_off)
FROM layoffs_staging2
GROUP BY company,YEAR(`date`)

), Company_Year_Rank AS
(
SELECT *, DENSE_RANK() OVER(PARTITION BY years ORDER BY total_laid_off DESC) AS RANKING
FROM Company_Year
WHERE years IS NOT NULL
)
SELECT *
FROM Company_Year_Rank
WHERE Ranking <= 5;

![image](https://github.com/Kajol0810/MySQL-Exploratory-Data-Analysis/assets/59485729/785e0121-bc52-4dbe-b8c8-5c62b7ddb1d9)
