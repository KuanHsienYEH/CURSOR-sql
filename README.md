# CURSOR
> select language which is not filled with 5. Make up all vacancy with data of selected language.**


```
DECLARE @ID int

DECLARE CategoryDetailCursor CURSOR FOR 
SELECT ID from WISE_PaaS_PlanCategoryDetail group by ID having COUNT(ID) < 5

OPEN CategoryDetailCursor
FETCH NEXT FROM CategoryDetailCursor INTO @ID

WHILE(@@FETCH_STATUS = 0)
BEGIN
 insert into WISE_PaaS_PlanCategoryDetail
 select @ID,LOWER(Code),
 (select top 1 DisplayName from WISE_PaaS_PlanCategoryDetail where ID = @ID and Lang ='zh-tw')
 ,null from WISE_PaaS_Culture where LOWER(Code) Not in (select LOWER(Lang) from WISE_PaaS_PlanCategoryDetail where ID = @ID)

 fetch NEXT FROM CategoryDetailCursor into @ID

END

CLOSE CategoryDetailCursor
deallocate CategoryDetailCursor
```
