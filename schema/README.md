
# HR & OE Schemas
> We are going to use Oracle sample schemas for Practise


### Refer to below links for more details
> [HR Schema](https://docs.oracle.com/cd/B13789_01/server.101/b10771/scripts003.htm)

> [OE Schema](https://docs.oracle.com/cd/B13789_01/server.101/b10771/scripts004.htm)


### Schema Diagrom

![image](https://github.com/venkatdurgempudi/SQL/assets/15828692/d32fcffd-1a88-474f-821e-efc0fc1b5894)

### Human Resources (HR)
- In the human resource records, each employee has an identification number, email
address, job identification code, salary, and manager. Some employees earn a
commission in addition to their salary.
- The company also tracks information about jobs within the organization. Each job
has an identification code, job title, and a minimum and maximum salary range for
the job. Some employees have been with the company for a long time and have held
different positions within the company. When an employee switches jobs, the
company records the start date and end date of the former job, the job identification
number, and the department.
- The sample company is regionally diverse, so it tracks the locations of not only its
warehouses but also of its departments. Each company employee is assigned to a
department. Each department is identified by a unique department number and a
short name. Each department is associated with one location. Each location has a
full address that includes the street address, postal code, city, state or province, and
country code.
- For each location where it has facilities, the company records the country name,
currency symbol, currency name, and the region where the county resides
geographically.

### Order Entry (OE)
- The company sells several categories of products, including computer hardware
and software, music, clothing, and tools. The company maintains information that
includes product identification numbers, the category into which the product falls,
the weight group (for shipping purposes), the warranty period if applicable, the
supplier, the availability status of the product, a list price, a minimum price at
which a product will be sold, and a URL address for manufacturer information.
Inventory information is also recorded for all products, including the warehouse
where the product is available and the quantity on hand. Because products are sold
worldwide, the company maintains the names of the products and their
descriptions in several languages.
- The company maintains warehouses in several locations to facilitate filling
customer orders. Each warehouse has a warehouse identification number, name,
facility description, and location identification number.
- Customer information is tracked in some detail. Each customer is assigned an
identification number. Customer records include name, street address, city or
province, country, phone numbers (up to five phone numbers for each customer),
and postal code. Some customers order through the Internet, so email addresses are
also recorded. Because of language differences among customers, the company
records the native language and territory of each customer.
- The company places a credit limit on its customers to limit the amount they can
purchase at one time. Some customers have an account manager, and this
information is also recorded.
- When a customer places an order, the company tracks the date of the order, how the
order was placed, the current status of the order, shipping mode, total amount of
the order, and the sales representative who helped place the order. The sales
representative may or may not be the same person as the account manager for a
customer. In the case of an order over the Internet, no sales representative is
recorded. In addition to the order information, the company also tracks the number
of items ordered, the unit price, and the products ordered.
- For each country in which it does business, the company records the country name,
currency symbol, currency name, and the region where the county resides
geographically. Thi

### Oracle PDF Material
> <a href="pdfs/database-sample-schemas.pdf" class="image fit"><img src="images/database-sample-schemas.png" alt=""></a>
