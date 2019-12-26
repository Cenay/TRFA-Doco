# Import Process from IFS to ActiveCampaign
For each "type" of customer record and event, we need to bring in the records from IFS. 

## Process
 * Query in IFS to get the "records" we need for the import of type
 * Tag each with "Export To AC" (so we can remove them once done)
 * Export them to CSV (all fields that are present in AC)
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

