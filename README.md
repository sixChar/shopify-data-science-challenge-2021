# shopify-data-science-challenge-2021
Submission for the Shopify fall 2021 Data Science intern challenge.


Question 1:
	For the detailed analysis see the file Q1_analysis.ipynb.
	
	a)
		The average order value is sensitive to the large outlier orders in the data.
		
	b)
		I would suggest using the median as a metric, possibly including the 25% and 75%
		quartiles to give a sense of the spread of the data if that is deemed relevant.
		
		Or if there was an important reason that the mean was used originally I would 
		suggest using the average of the order values in the interdecile range (10% to 
		90%) to cut out the outliers.
		
		If there is a strong desire to have a more continuous metric than the median 
		without cutting out any of the data I might suggest using e^mean(log(x)) where x 
		is the order values as the primary metric. I would also suggest tracking the 
		median to make sure the above metric doesn't get heavily skewed if even more 
		extreme outliers enter the data later.
		
	c)
		The values of each of these metrics (to 2 decimal places) are as follows:
		Median: 284.00
		25% quartile: 163.00
		75% quartile: 390
		
		Interdecial Average Order Value: 287.66
		
		e^mean(log(x)): 285.02
		
Question 2:

	a)
	
		SELECT COUNT(ord.OrderID)
		FROM Shippers AS shp, Orders AS ord
		WHERE shp.ShipperName='Speedy Express'
		AND shp.ShipperId=ord.ShipperID;
	
	b)
		SELECT LastName FROM
		(SELECT TOP 1 EmployeeID, COUNT(EmployeeID)
		FROM Orders 
		GROUP BY EmployeeID
		ORDER BY COUNT(EmployeeID) DESC) AS maxEmp, Employees AS emp
		WHERE maxEmp.EmployeeID=emp.EmployeeID;
		
	c)
		SELECT ProductName 
		FROM (
			SELECT TOP 1 ProductID, SUM(Quantity)
			FROM OrderDetails AS odt, Orders AS ord, Customers AS cus
			WHERE odt.OrderID=ord.OrderID 
			AND cus.CustomerID=ord.CustomerID 
			AND cus.Country='Germany'
			GROUP BY ProductID
			ORDER BY SUM(Quantity) DESC
			) AS topPrd, Products AS prd
		WHERE topPrd.ProductID = prd.ProductID;





