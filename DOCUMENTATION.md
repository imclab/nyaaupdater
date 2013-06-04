Documentation
===========

## nyaa_parser.py
Fetching and parsing nyaa.eu html page.

library functions:
*	fetch(url, regexPattern)
	connect to url (nyaa.eu search url) and fetch result by regexPattern using NyaaParser
	the result will be a list of tuple (filename, link)
*	download(url, filename)
	download from url as filename

Using NyaaParser on your own:
1. 	build a NyaaParser object
	parser = NyaaParser(regexPattern [, parseAll=True])
	
	regexPattern is used to identify which search result should be taken
	parseAll is used to check whether fetch all result or not. if it is False, NyaaParser only fetch the first result.
	
2.	feed with a nyaa.eu search result page (html)
	parser.feed(nyaaPage)
	
3.	fetch the result
	result = parser.result
	the result will be a list of tuple (filename, link)

## nyaa_db.py
Manages feed database.

The feed database (nyaa_checklist.csv) is a comma-seperated values of:
series_id, search_url, regex_pattern, last_downloaded

series_id : the series name
search_url : nyaa.eu search url
regex_pattern : regex pattern of desired result
last_downloaded: the last downloaded file's name, without .torrent extension

usages:
1.	building a NyaaDB object. you may use a custom file as long as it is consistent with the format above
	db = NyaaDB()
2. 	load the database
	db.load()
	it will return a dictionary of key series_id and values list [search_url, regex_pattern, last_downloaded]. ex: {series1: [url, pattern, filename]}
	you can also access this list using db.data
	
3.	add, updates and delete
	db.add(new_data)
	add new_data to database
	new_data is a dictionary of key series_id and values list [search_url, regex_pattern, last_downloaded]. ex: {series1: [url, pattern, filename]}
	
	db.delete(keys)
	delete entries with key keys from database
	keys is list of series_id. ex: [series1, series2, series3]
	
	db.update(updates)
	update database with updates
	updates is a dictionary of key series_id and values list [search_url, regex_pattern, last_downloaded]. ex: {series1: [url, pattern, filename]}
	if url, pattern or filename is not empty / None, it will be updated to the database
	please make sure the last_downloaded is the last downloaded file's name, without .torrent extension
	
4.	write changes to database
	db.write()
	write changes to database file. write() is automatically called by add, delete and update