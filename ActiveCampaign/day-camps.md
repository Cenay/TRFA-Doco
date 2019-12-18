# DAY CAMPS
We are changing the way we handle the day camps. They will now be named, separate entries with thier own dates. They will each have the - Day Camp suffix added. (ie: President's Day becomes PRESIDENT'S DAY $85 - Day Camp). 

## Notes from our intial meeting
Previously, it was a "scheduled" time frame, limiting Art's options. We are changing the daily camps to mimic the Kids Class method. Each new Day Camp will have a unique product id, code, date, etc. For those that are of a single date per year, the suffix will be Day Camp.   

UPDATE: Daily options for Summer and Winter camp will remain the same as they are now.   

For the day camp's there will be a set of hard coded tags for the initial "type" of event, and the API should extract the "name" of the camp (ie: PRESIDENT'S DAY) and add tag for it. To do this, take the title of the camp, remove the - Day Camp, convert the remainder to Camel Case and insert the trimmed result as a tag. (This should create the tag if not found on the fly, let me know if it doesn't)  

Hard Coded Tags for events that have the Day Camp suffix
 * 184 (Kids Camp . Daily)
 * 186 (Kids Camp . Purchased)

Create new tag on the fly that is the modified title. (See notes above)


