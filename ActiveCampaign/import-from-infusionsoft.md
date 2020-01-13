# Import Process from IFS to ActiveCampaign
For each "type" of customer record and event, we need to bring in the records from IFS. 

## Process
 * Query in IFS to get the "records" we need for the import of type
 * Tag each with "Export To AC" (so we can remove them once done)
 * Export them to CSV (use template "Just To AC")
 * Perform some basic clean up on the records. 
   * Remove all null values
   * Remove the temp type tags
   * Determine any special conditions to edit on the fly
   * When data is clean, import into temp table and compare against master list of AC records (we need to know which one's need special handling [ie: record already exists in AC and there is old info to bring in])
     * To Create the Temp Table: 
       * https://www.convertcsv.com/csv-to-sql.htm and pull in table
   * From two queries (those that exist in AC and those that don't), create two CSVs
   * Import those that don't exist immediately.
   * Import those that do exist to another table for later processing  

>This process is being revised. More information when that's done.  


## Tool Created
To help pull in all the records, clean the data, format and remove extraneous content, I created an app to run locally called processcsv (/www/processcsv/).  Accepts a CSV, allows a field mapping from CSV column headers to database headers, and then pulls in the data, formatting and cleaning on the way.  