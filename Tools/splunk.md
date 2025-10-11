
Search Query: index=botsv1 imreallynotbatman.com
Search Query explanation: We are going to look for the event logs in the index "botsv1" which contains the term imreallynotbatman.com

Search Query: index=botsv1 imreallynotbatman.com sourcetype=stream:http
Search Query Explanation: This query will only look for the term  imreallynotbatman.comin the stream:http log source


Search Query: index=botsv1 imreallynotbatman.com src=40.80.148.42 sourcetype=suricata
Search Query Explanation: This query will show the logs from the suricata log source that are detected/generated from the source IP 40.80.248.42


Search Query:index=botsv1 imreallynotbatman.com sourcetype=stream* | stats count(src_ip) as Requests by src_ip | sort - Requests
Query Explanation: This query uses the stats function to display the count of the IP addresses in the field src_ip.


Search Query: index=botsv1 sourcetype=stream:http dest_ip="192.168.250.70"
Query Explanation: This query will look for all the inbound traffic towards IP 192.168.250.70.

To see what kind of traffic is coming through the POST requests, we will narrow down on the field http_method=POST as shown below:
Search Query: index=botsv1 sourcetype=stream:http dest_ip="192.168.250.70" http_method=POST


Search query: index=botsv1 imreallynotbatman.com sourcetype=stream:http dest_ip="192.168.250.70"  uri="/joomla/administrator/index.php"
Query Explanation: We are going to add uri="/joomla/administrator/index.php" in the search query to show the traffic coming into this URI.


Search Query: index=botsv1 sourcetype=stream:http dest_ip="192.168.250.70" http_method=POST uri="/joomla/administrator/index.php" | table _time uri src_ip dest_ip form_data
Query Explanation: We will add this -> | table _time uri src_ip dest_ip form_data to create a table containing important fields as shown below: 

We can display only the logs that contain the username and passwd values in the form_data field by adding form_data=*username*passwd* in the above search.
Search Query: index=botsv1 sourcetype=stream:http dest_ip="192.168.250.70" http_method=POST uri="/joomla/administrator/index.php" form_data=*username*passwd* | table _time uri src_ip dest_ip form_data


Now, let's use Regex.  rex field=form_data "passwd=(?<creds>\w+)" To extract the passwd values only. This will pick the form_data field and extract all the values found with the field. creds.
Search Query:index=botsv1 sourcetype=stream:http dest_ip="192.168.250.70" http_method=POST form_data=*username*passwd* | rex field=form_data "passwd=(?<creds>\w+)"  | table src_ip creds


Search Query: index=botsv1 sourcetype=stream:http dest_ip="192.168.250.70" http_method=POST form_data=*username*passwd* | rex field=form_data "passwd=(?<creds>\w+)" |table _time src_ip uri http_user_agent creds
