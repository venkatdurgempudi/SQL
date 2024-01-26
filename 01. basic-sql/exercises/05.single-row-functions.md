<!--
select rd.*,DELIVERY_DATE - ORDER_DATE  from 
-- truncate table
random_data rd
order by order_id
;

select extract(year from ORDER_DATE) year
,to_char(ORDER_DATE,'Q') as Qtr
--, extract(month from ORDER_DATE) month
,count(order_id) order_count
,sum(amount) total_amount
from random_data
group by extract(year from ORDER_DATE)
,to_char(ORDER_DATE,'Q')
--, extract(month from ORDER_DATE)
order by extract(year from ORDER_DATE)
,to_char(ORDER_DATE,'Q')
--, extract(month from ORDER_DATE)
;

select order_id,count(*) from 
random_data 
group by order_id
having count(*) = 1
order by  1

select order_id,avg(amount),floor(avg(amount)) from 
random_data 
group by order_id
--having count(*) = 1
order by  1
-->

### Pre-requisite:  

- Execute the following script for data.  

```sql
CREATE TABLE random_data (
	ORDER_ID NUMBER
	,ORDER_DESC VARCHAR2(4000)
	,ORDER_TYPE VARCHAR2(4000)
	,ORDER_DATE DATE
	,DELIVERY_DATE DATE
	,AMOUNT NUMBER(4)
	);
	

Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (006100,'bZEkQGtw','B',to_date('19-FEB-23','DD-MON-RR'),to_date('31-AUG-23','DD-MON-RR'),0);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (7400,'mGWUm','G',to_date('04-APR-22','DD-MON-RR'),to_date('20-APR-22','DD-MON-RR'),-0850);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (2700,'ENpRosC','Y',to_date('20-JUN-22','DD-MON-RR'),to_date('14-MAY-23','DD-MON-RR'),0);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (6400,'DPtuQRZIu','Q',to_date('20-APR-23','DD-MON-RR'),to_date('24-FEB-24','DD-MON-RR'),-000720);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (7200,'jzFdSah','T',to_date('19-JAN-24','DD-MON-RR'),to_date('26-SEP-25','DD-MON-RR'),00710);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (200,'rUBqWPOC','T',to_date('01-AUG-22','DD-MON-RR'),to_date('03-FEB-24','DD-MON-RR'),-00780);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (001000,'jVbvYs','F',to_date('12-SEP-22','DD-MON-RR'),to_date('15-JUL-23','DD-MON-RR'),0);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (2600,'kCUueHl','Z',to_date('30-SEP-22','DD-MON-RR'),to_date('01-FEB-23','DD-MON-RR'),260);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (9100,'rYqhEQNn','O',to_date('09-NOV-22','DD-MON-RR'),to_date('10-MAY-23','DD-MON-RR'),300);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (1300,'QUEwls','F',to_date('12-NOV-23','DD-MON-RR'),to_date('05-MAR-25','DD-MON-RR'),-00330);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (4200,'LPLGRN   ','T',to_date('01-MAY-22','DD-MON-RR'),to_date('19-JAN-24','DD-MON-RR'),-780);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (4700,'DlqwPJl','Z',to_date('26-JUL-22','DD-MON-RR'),to_date('23-JUN-23','DD-MON-RR'),420);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (0002500,'IRpsNlBwP','P',to_date('15-FEB-22','DD-MON-RR'),to_date('19-MAR-22','DD-MON-RR'),0);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (3100,'cfpTGut','A',to_date('21-MAR-23','DD-MON-RR'),to_date('22-JUN-23','DD-MON-RR'),-910);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (8600,'DaCXcvJ','S',to_date('26-AUG-23','DD-MON-RR'),to_date('14-JAN-25','DD-MON-RR'),0);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (2500,'EDnGWrobd','D',to_date('03-MAR-23','DD-MON-RR'),to_date('29-AUG-23','DD-MON-RR'),0);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (8600,'miORLDTs','U',to_date('25-FEB-23','DD-MON-RR'),to_date('26-AUG-23','DD-MON-RR'),0);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (800,'pauWtBP','V',to_date('09-APR-22','DD-MON-RR'),to_date('12-FEB-24','DD-MON-RR'),-250);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (9700,'fnvIOdY','Y',to_date('20-OCT-23','DD-MON-RR'),to_date('27-JUL-24','DD-MON-RR'),0);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (1100,'ufaKyh','K',to_date('17-FEB-22','DD-MON-RR'),to_date('09-FEB-24','DD-MON-RR'),0);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (1600,'Gjpkgswzy',',',to_date('18-DEC-23','DD-MON-RR'),to_date('01-JUL-25','DD-MON-RR'),0);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (3700,'wzuooCTL','O',to_date('24-APR-22','DD-MON-RR'),to_date('30-AUG-22','DD-MON-RR'),-280);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (9900,'lXuexzKQo','n',to_date('26-JAN-24','DD-MON-RR'),to_date('02-OCT-25','DD-MON-RR'),0);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (2000,'yKGyc','3',to_date('18-MAR-22','DD-MON-RR'),to_date('03-JUL-22','DD-MON-RR'),0);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (3800,'goPkd','D',to_date('17-NOV-23','DD-MON-RR'),to_date('14-JAN-25','DD-MON-RR'),0);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (5100,'GfIhn',',',to_date('20-JAN-24','DD-MON-RR'),to_date('02-JAN-25','DD-MON-RR'),-970);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (100,'   SMBJkCP','U',to_date('10-FEB-23','DD-MON-RR'),to_date('13-SEP-23','DD-MON-RR'),0);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (2700,'ouIrUZSR','+',to_date('10-OCT-23','DD-MON-RR'),to_date('04-JUL-24','DD-MON-RR'),0);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (9300,'CHDvmShy','g',to_date('04-NOV-23','DD-MON-RR'),to_date('04-NOV-24','DD-MON-RR'),00170);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (9500,'woqvZR','.',to_date('06-JAN-23','DD-MON-RR'),to_date('24-MAY-23','DD-MON-RR'),00110);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (3100,'GVMJlLsF','p',to_date('16-DEC-23','DD-MON-RR'),to_date('08-AUG-25','DD-MON-RR'),-770);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (3300,'XheGLQ','.',to_date('04-AUG-22','DD-MON-RR'),to_date('10-OCT-22','DD-MON-RR'),0);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (4500,'BUISsK','|',to_date('10-MAR-23','DD-MON-RR'),to_date('09-NOV-23','DD-MON-RR'),0);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (1900,'tEbtcNDNd','J',to_date('03-JUL-23','DD-MON-RR'),to_date('17-MAY-24','DD-MON-RR'),-30);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (7200,'cxelOdte','=',to_date('10-MAR-23','DD-MON-RR'),to_date('30-MAY-23','DD-MON-RR'),580);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (4700,'aIQmFAo','>',to_date('03-DEC-22','DD-MON-RR'),to_date('18-DEC-22','DD-MON-RR'),0);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (3300,'AaFpvqoCM','6',to_date('18-AUG-23','DD-MON-RR'),to_date('05-JUL-24','DD-MON-RR'),-610);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (1800,'RSDoLdPJ','M',to_date('24-MAR-22','DD-MON-RR'),to_date('05-APR-22','DD-MON-RR'),0);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (500,'OZloji','Q',to_date('08-APR-22','DD-MON-RR'),to_date('28-SEP-22','DD-MON-RR'),450);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (600,'nOnZSXS','C',to_date('11-MAR-23','DD-MON-RR'),to_date('29-MAR-23','DD-MON-RR'),980);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (1100,'SZdHrNF','u',to_date('19-FEB-22','DD-MON-RR'),to_date('12-MAR-23','DD-MON-RR'),-0480);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (8900,'GNLUwr','z',to_date('31-MAR-22','DD-MON-RR'),to_date('05-MAR-23','DD-MON-RR'),020);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (6300,'   RWeBYprLS','n',to_date('04-AUG-23','DD-MON-RR'),to_date('04-JAN-24','DD-MON-RR'),0);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (3400,'DUhbtn','v',to_date('04-SEP-22','DD-MON-RR'),to_date('06-MAR-23','DD-MON-RR'),0);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (4300,'  DnHIqAm   ','s',to_date('28-NOV-23','DD-MON-RR'),to_date('19-SEP-24','DD-MON-RR'),940);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (9800,'hUzmfd  ','i',to_date('22-JAN-23','DD-MON-RR'),to_date('02-MAR-23','DD-MON-RR'),0);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (8500,'  MYEAtFFLx  ','a',to_date('27-OCT-22','DD-MON-RR'),to_date('01-MAR-23','DD-MON-RR'),-170);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (3800,'bWRXBCa','j',to_date('27-MAY-22','DD-MON-RR'),to_date('15-FEB-23','DD-MON-RR'),0);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (6100,'BTuzf','v',to_date('07-NOV-23','DD-MON-RR'),to_date('02-JUL-24','DD-MON-RR'),-310);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (9700,'  sFAmKoYOC  ','x',to_date('17-OCT-22','DD-MON-RR'),to_date('27-FEB-23','DD-MON-RR'),-520);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (1100,'KUnkIuZ','f',to_date('23-MAR-23','DD-MON-RR'),to_date('30-APR-23','DD-MON-RR'),170);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (8500,'VSWTbS  ','f',to_date('08-OCT-23','DD-MON-RR'),to_date('26-APR-24','DD-MON-RR'),600);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (8900,'DKxjGsF','j',to_date('08-JAN-24','DD-MON-RR'),to_date('26-OCT-24','DD-MON-RR'),0);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (7600,'yJVSty','d',to_date('19-NOV-23','DD-MON-RR'),to_date('04-AUG-24','DD-MON-RR'),320);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (5800,'EdSETg','z',to_date('07-JUN-23','DD-MON-RR'),to_date('30-AUG-23','DD-MON-RR'),0);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (200,'EfwKsz','i',to_date('27-APR-23','DD-MON-RR'),to_date('12-JUN-23','DD-MON-RR'),920);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (8800,'Hsytcz ','h',to_date('05-MAR-23','DD-MON-RR'),to_date('04-APR-23','DD-MON-RR'),0);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (2200,'  lrRgso','b',to_date('06-SEP-23','DD-MON-RR'),to_date('24-MAR-24','DD-MON-RR'),-440);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (5600,'yTYZWYZL','i',to_date('02-JAN-24','DD-MON-RR'),to_date('18-OCT-24','DD-MON-RR'),0);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (6200,'MKvMdqXO   ','s',to_date('02-SEP-23','DD-MON-RR'),to_date('13-MAR-24','DD-MON-RR'),0);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (3900,'5FX37F  ','z',to_date('14-JAN-23','DD-MON-RR'),to_date('14-MAR-23','DD-MON-RR'),0);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (4900,'E54CMLTR0','t',to_date('29-JUL-22','DD-MON-RR'),to_date('04-MAR-23','DD-MON-RR'),0);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (3900,'QSUWKWF','h',to_date('29-AUG-23','DD-MON-RR'),to_date('10-MAR-24','DD-MON-RR'),30);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (8900,'NQ5BE','H',to_date('09-APR-22','DD-MON-RR'),to_date('16-MAR-23','DD-MON-RR'),-290);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (6600,'SN9MRUD','z',to_date('09-SEP-22','DD-MON-RR'),to_date('13-FEB-23','DD-MON-RR'),940);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (5600,'ATL6H0','O',to_date('02-OCT-22','DD-MON-RR'),to_date('22-MAR-23','DD-MON-RR'),0);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (800,'W1BI1WJ    ','G',to_date('01-MAR-23','DD-MON-RR'),to_date('12-MAR-23','DD-MON-RR'),0);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (7400,'6N2KGGXJR','t',to_date('04-JUL-23','DD-MON-RR'),to_date('06-NOV-23','DD-MON-RR'),0);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (3700,'NK7KA   ','O',to_date('16-NOV-23','DD-MON-RR'),to_date('17-AUG-24','DD-MON-RR'),0);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (4000,'ESW43Z9','D',to_date('12-JAN-23','DD-MON-RR'),to_date('17-MAR-23','DD-MON-RR'),0);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (2100,'   RKAMLOPPG','z',to_date('03-DEC-22','DD-MON-RR'),to_date('25-FEB-23','DD-MON-RR'),-410);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (6300,'2K2HDDL0','B',to_date('22-JUL-23','DD-MON-RR'),to_date('24-NOV-23','DD-MON-RR'),-530);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (1700,'   K2E4I6W','z',to_date('11-APR-23','DD-MON-RR'),to_date('14-MAY-23','DD-MON-RR'),0);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (1700,'TAJR6KB','W',to_date('05-OCT-23','DD-MON-RR'),to_date('15-MAY-24','DD-MON-RR'),-620);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (2200,'SA7S29S','e',to_date('17-AUG-22','DD-MON-RR'),to_date('09-FEB-23','DD-MON-RR'),0);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (3100,'M7TQH','i',to_date('05-DEC-23','DD-MON-RR'),to_date('20-SEP-24','DD-MON-RR'),-630);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (2000,'CGDZY','a',to_date('29-JUN-22','DD-MON-RR'),to_date('21-FEB-23','DD-MON-RR'),980);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (7200,'ZWSPBFL4E','z',to_date('31-JAN-24','DD-MON-RR'),to_date('15-JAN-25','DD-MON-RR'),0);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (8200,'3QUEI','m',to_date('11-APR-23','DD-MON-RR'),to_date('12-MAY-23','DD-MON-RR'),-890);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (300,'   ABZZ5','B',to_date('29-NOV-22','DD-MON-RR'),to_date('18-MAR-23','DD-MON-RR'),590);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (100,'3G23C   ','V',to_date('06-JUN-22','DD-MON-RR'),to_date('06-JAN-24','DD-MON-RR'),240);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (200,'S70B5BQ0   ','D',to_date('09-FEB-22','DD-MON-RR'),to_date('02-JAN-24','DD-MON-RR'),970);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (200,'FGKW4GWH','r',to_date('11-JUN-22','DD-MON-RR'),to_date('08-JAN-24','DD-MON-RR'),370);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (400,'81IT8B','H',to_date('08-SEP-22','DD-MON-RR'),to_date('14-JAN-24','DD-MON-RR'),0);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (200,'GXFKTVIO8','t',to_date('01-APR-22','DD-MON-RR'),to_date('16-JAN-24','DD-MON-RR'),0);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (300,'YSYMEC9','J',to_date('12-NOV-22','DD-MON-RR'),to_date('09-JAN-24','DD-MON-RR'),-760);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (200,'MPE1TJ1Q','j',to_date('04-FEB-23','DD-MON-RR'),to_date('04-JAN-24','DD-MON-RR'),0);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (400,'D8N3R199J','H',to_date('21-APR-22','DD-MON-RR'),to_date('30-DEC-23','DD-MON-RR'),10);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (400,'VAGKXP2','v',to_date('25-SEP-23','DD-MON-RR'),to_date('03-JAN-24','DD-MON-RR'),730);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (200,'DPRZM','S',to_date('12-APR-23','DD-MON-RR'),to_date('01-JAN-24','DD-MON-RR'),0);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (300,'ZCSTKWDFB   ','w',to_date('01-MAY-23','DD-MON-RR'),to_date('01-JAN-24','DD-MON-RR'),270);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (200,'   Q6KY54Q1','J',to_date('06-JUN-23','DD-MON-RR'),to_date('06-JAN-24','DD-MON-RR'),650);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (300,'IGVM8D','g',to_date('24-DEC-22','DD-MON-RR'),to_date('02-JAN-24','DD-MON-RR'),880);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (100,'9WATG','J',to_date('14-JUL-22','DD-MON-RR'),to_date('16-JAN-24','DD-MON-RR'),-820);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (200,'YP550AIY  ','r',to_date('11-SEP-22','DD-MON-RR'),to_date('15-JAN-24','DD-MON-RR'),0);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (200,'4UW1Y6','t',to_date('14-DEC-22','DD-MON-RR'),to_date('16-JAN-24','DD-MON-RR'),-320);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (100,'FEN0EC7','B',to_date('22-MAY-23','DD-MON-RR'),to_date('06-JAN-24','DD-MON-RR'),280);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (400,'  19L7FL1','l',to_date('02-FEB-23','DD-MON-RR'),to_date('05-JAN-24','DD-MON-RR'),120);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (200,'UXO58   ','c',to_date('01-JUN-23','DD-MON-RR'),to_date('02-JAN-24','DD-MON-RR'),830);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (400,'MFBLX7  ','u',to_date('16-JUN-22','DD-MON-RR'),to_date('09-JAN-24','DD-MON-RR'),0);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (100,'lxosraul  ','v',to_date('27-DEC-23','DD-MON-RR'),to_date('07-JAN-24','DD-MON-RR'),-220);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (200,'phuuu','l',to_date('29-DEC-23','DD-MON-RR'),to_date('12-JAN-24','DD-MON-RR'),0);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (200,'qaxdqznu','T',to_date('29-DEC-23','DD-MON-RR'),to_date('01-JAN-24','DD-MON-RR'),0);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (200,'efavc','u',to_date('29-DEC-23','DD-MON-RR'),to_date('11-JAN-24','DD-MON-RR'),0);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (100,'oxoyzf','c',to_date('28-DEC-23','DD-MON-RR'),to_date('12-JAN-24','DD-MON-RR'),80);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (300,'zhkmjsnp','A',to_date('29-DEC-23','DD-MON-RR'),to_date('09-JAN-24','DD-MON-RR'),0);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (400,'vrcuao','j',to_date('28-DEC-23','DD-MON-RR'),to_date('16-JAN-24','DD-MON-RR'),-810);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (000300,'azqwez','f',to_date('28-DEC-23','DD-MON-RR'),to_date('31-DEC-23','DD-MON-RR'),-810);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (400,'  mjtva','K',to_date('28-DEC-23','DD-MON-RR'),to_date('09-JAN-24','DD-MON-RR'),0);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (000100,'pdkmf','j',to_date('28-DEC-23','DD-MON-RR'),to_date('05-JAN-24','DD-MON-RR'),590);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (200,'  pspxp','q',to_date('28-DEC-23','DD-MON-RR'),to_date('05-JAN-24','DD-MON-RR'),940);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (200,'  vpbxa','L',to_date('27-DEC-23','DD-MON-RR'),to_date('11-JAN-24','DD-MON-RR'),0);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (400,'jdczr','B',to_date('29-DEC-23','DD-MON-RR'),to_date('31-DEC-23','DD-MON-RR'),820);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (200,'cgievxrrr','g',to_date('28-DEC-23','DD-MON-RR'),to_date('28-DEC-23','DD-MON-RR'),400);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (200,'lxknowd','j',to_date('28-DEC-23','DD-MON-RR'),to_date('15-JAN-24','DD-MON-RR'),0);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (300,'azhkhe','u',to_date('29-DEC-23','DD-MON-RR'),to_date('09-JAN-24','DD-MON-RR'),-920);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (300,'  ajneurnwc','e',to_date('29-DEC-23','DD-MON-RR'),to_date('03-JAN-24','DD-MON-RR'),970);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (100,'ifymwwy  ','z',to_date('27-DEC-23','DD-MON-RR'),to_date('04-JAN-24','DD-MON-RR'),-540);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (300,'  bgixubtqu  ','b',to_date('28-DEC-23','DD-MON-RR'),to_date('06-JAN-24','DD-MON-RR'),-570);
Insert into random_data (ORDER_ID,ORDER_DESC,ORDER_TYPE,ORDER_DATE,DELIVERY_DATE,AMOUNT) values (100,'  ibvsn','M',to_date('29-DEC-23','DD-MON-RR'),to_date('09-JAN-24','DD-MON-RR'),-630);
commit;
```

### Column info:  

- Order_Desc column is having Uppercase/Lower case/alpha-nuemeric data.
- Order_Type column is having Uppercase/Lower case/nuemeric/printable character data including =+.>


### Assessment:   



> [!CAUTION]
> The Order_Desc column may contains Trailing and Leading Whitespaces that need to be removed while processing the data. 

<br>  


1. Please write select statement to produce records whose Order_Desc is in Upper case
   
    ```if numbers are there, they can also be considered```
1. Please write select statement to produce records whose Order_Desc do not have any numbers
   
    ```
   hint:
    Can use Nested replace to replace 0-9 digits and compare string to original
    ```
1. Please write select statement to produce records whose Order_Desc starting with number (0-9)
   
    ```
   hint:
    can use substr to extract 1st character and compare it with 0-9 digits
    ```
1. Find the Max length of order_desc
1. Write SQL to produce records whose Order_Type (single character) is exist with in Order_Desc atleast once
   
   ```case sensitive```  
 
   ```
    example:
    Order_Id 2500 Order_Desc 'IRpsNlBwP' has Order_Type 'P' in it at the end.
   ```
1. Write SQL to produce records whose Order_Type (single character) is exist with in Order_Desc atleast once
   
   ```case in-sensitive```
   
   ```example: including above example in e Order_Id 300 Order_Desc 'ZCSTKWDFB' has Order_Type 'w' in it.```  
1. Write SQL to produce records whose last character Order_Desc is same as Order_type
  
   ```Case insensitive```
1. Write SQL to produce records whose Order_Type is not lower case or Upper case.
   ```
   example: 
   7200	cxelOdte	= 
   3300	AaFpvqoCM	6
   ```  
   ```
    applying lower/upper on values such as 6 or + or similar, will not change anything.  
    so '6' = upper('6') = lower('6')
   ```  
1. List the records whose Delivery_Date is within 30 Days from Order_date.
1. List the records whose Delivery_Date is within 3 months from Order_date.
1. List the records whose Delivery_Date and Order_date is on same day
    ```
    Example:    Both on 4th
    200	MPE1TJ1Q	j	04-FEB-23	04-JAN-24
    ```
1. List the records whose Delivery_Date and Order_date happened in same quarter of the same Year.
1. List the records whose Delivery_Date and Order_date happened in same quarter Even if Year is different
1. List the records whose Delivery_Date is just one day before end of that month.
    ```
    Example: 31-May is last day of May, delivery happened just a day before on 30-May
    7200	cxelOdte	=	10-MAR-23	30-MAY-23
    ```

1. List the records whose overall amount is positive.
    ```
    Example:
    Order Id 100 has total amount -1020  (this should not come)
    Order Id 200	has total amount 3980 (this should come)
    ```     
1. Pleasr find the age of the Order (which is number of days it took to deliver from the date of Order in Days)
    ```
    Example:
    below order took 215 days after Order date to get Delivered
    100	SMBJkCP	U	10-FEB-23	13-SEP-23	0
    ```



1. Display amount in 9,99 format
    ```
    Example:
    amount -330 should reflect as -3,30
    ```

1. Display amount in $999 format
    ```
    Example:
    amount -330 should reflect as -$330
    ```

1. List the Orders that are delivered on the second day of Month
    ```
    Example: 
    9800	hUzmfd	i	22-JAN-23	02-MAR-23
    ```

1. Populate the Week of the Year that Order is delivered.
    ```
    Example: below record delivered on 35th Week of Year
    6100	bZEkQGtw	B	19-FEB-23	31-AUG-23	0
    ```

1. Please provide year wise, Quarter Wise, Month Wise number of orders and total amounts based on Order date.
    ```
    Example:
    "YEAR"	"QTR"	"ORDER_COUNT"	"TOTAL_AMOUNT"
    2022	"1"	        7	        510
    2022	"2"	        14	        -400
    2022	"3"	        12	        20
    2022	"4"	        10	        -410
    ```


1. display the delievery Date and Order Date in 'YYYY-MM-DD'
    ```
    Example:
    100	SMBJkCP	U	'2023-02-10'	'2023-09-13'
    ```


1. Display the Order_ids that are having only one Order Desc mapped to them.
    ```
    Example:
      ORDER_ID
      500
      600
      1000
    ```
1. Display the Average of Total for Each Order. Avarge should not have any decimal places and it should be rounded up
    ```
    Example:
    Order_Id: 100	actual average(-113.33) desired value(-114)
    Order_Id: 200	actual average(221.11)	desired value(221)
    ```



