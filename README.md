# SQL-Server-String-Split-Function
SQL Server String Split Function

```tsql
/****** Object:  UserDefinedFunction [dbo].[fn_Split_String]    Script Date: 6/13/2022 9:51:47 AM ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO

ALTER FUNCTION [dbo].[fn_Split_String](@varInputString VARCHAR(MAX), @varSeparator VARCHAR(10)) RETURNS @tblSplitStrings TABLE ([Value] VARCHAR(MAX)) 
AS
/***************************************************************************************************************************
** Parameters:          @varInputString - The string to perform splitting upon.
**                      @varSeparator - The separator to identify the split point.
**
** Description:         Returns a table of records where the input string has been split based on the separator.
**
** Programmer:          Deep Grewal
**
** Date Written:        06/10/2022
**
** Change History:      See Modification History below.
**
** Usage:
** SELECT fn_Split_String(@varInputString, @varSeparator)
**
***************************************************************************************************************************/
/**[ MODIFICATION HISTORY ]*************************************************************************************************
YYYY.MM.DD - First Last: (1) Numbered list of changes.
***************************************************************************************************************************/

BEGIN

    DECLARE @varSubString VARCHAR(MAX)
    DECLARE @intPos INT = 0
    DECLARE @intLen INT = LEN(@varInputString)  
    DECLARE @intCharIndex INT = CHARINDEX(@varSeparator, @varInputString, @intPos)

    IF @intLen = 0 RETURN

    WHILE @intPos <= @intLen
        BEGIN
            SET @intCharIndex = CHARINDEX(@varSeparator, @varInputString, @intPos)
            IF @intCharIndex > 0
                BEGIN
                    INSERT INTO @tblSplitStrings
                        SELECT SUBSTRING(@varInputString, @intPos, @intCharIndex - @intPos)
                    SET @intPos = @intCharIndex + LEN(@varSeparator)
                END
            ELSE
                BEGIN
                    INSERT INTO @tblSplitStrings
                        SELECT SUBSTRING(@varInputString, @intPos, @intLen + 1)
                    SET @intPos = @intLen + 1
                END
        END

    RETURN

END
```
