# Splunk `index=botsv1` — Quick Cheatsheet

## Notes
- These examples target the `botsv1` index.  
- Replace domain, IPs, URIs, and field names to match your environment.  
- Use `| table` to format output and `rex` to extract fields.  
- Only search systems you are authorized to query.

---

## Basic searches

**Find events containing the domain**
```spl
index=botsv1 imreallynotbatman.com
```

**Same, limited to HTTP stream logs**
```spl
index=botsv1 imreallynotbatman.com sourcetype=stream:http
```

**Suricata events from a specific source IP**
```spl
index=botsv1 imreallynotbatman.com src=40.80.148.42 sourcetype=suricata
```

---

## Aggregation / Counting

**Count requests per source IP and sort descending**
```spl
index=botsv1 imreallynotbatman.com sourcetype=stream* 
| stats count(src_ip) as Requests by src_ip 
| sort - Requests
```

---

## Traffic to a destination IP

**All HTTP traffic to destination IP**
```spl
index=botsv1 sourcetype=stream:http dest_ip="192.168.250.70"
```

**Only POSTs to that destination**
```spl
index=botsv1 sourcetype=stream:http dest_ip="192.168.250.70" http_method=POST
```

---

## Narrow to a specific URI (example: Joomla admin)

**Requests to a specific URI**
```spl
index=botsv1 imreallynotbatman.com sourcetype=stream:http dest_ip="192.168.250.70" uri="/joomla/administrator/index.php"
```

**POSTs to that URI in a table**
```spl
index=botsv1 sourcetype=stream:http dest_ip="192.168.250.70" http_method=POST uri="/joomla/administrator/index.php"
| table _time uri src_ip dest_ip form_data
```

---

## Form-data inspection & extraction

**Only logs where `form_data` contains both `username` and `passwd`**
```spl
index=botsv1 sourcetype=stream:http dest_ip="192.168.250.70" http_method=POST uri="/joomla/administrator/index.php" form_data=*username*passwd*
| table _time uri src_ip dest_ip form_data
```

**Extract passwd value into field `creds` and show source IP**
```spl
index=botsv1 sourcetype=stream:http dest_ip="192.168.250.70" http_method=POST form_data=*username*passwd*
| rex field=form_data "passwd=(?<creds>\w+)"
| table src_ip creds
```

**Table: time, src_ip, uri, user agent, and extracted creds**
```spl
index=botsv1 sourcetype=stream:http dest_ip="192.168.250.70" http_method=POST form_data=*username*passwd*
| rex field=form_data "passwd=(?<creds>\w+)"
| table _time src_ip uri http_user_agent creds
```

---

## Quick tips*.
- Use `sourcetype=stream*` to match multiple stream sourcetypes if needed.  
- Adjust the regex to allow special characters in passwords:  
  `"passwd=(?<creds>[^&]+)"`  
- Use `| head 100` or set a time range in Splunk to limit results.

index=botsv1 sourcetype=stream:http dest_ip="192.168.250.70" *.exe

For the evidence of execution, we can leverage sysmon and look at the EventCode=1 for program execution.
sysmon
WinEventlog

Search Query: index=botsv1 "3791.exe" sourcetype="XmlWinEventLog" EventCode=1
Query Explanation: This query will look for the process Creation logs containing the term "3791.exe" in the logs.

index=botsv1 dest=192.168.250.70 sourcetype=suricata


The URL field shows 2 PHP files and one jpeg file. This jpeg file looks interesting. Let us change the search query and see where this jpeg file came from.
Search Query: index=botsv1 url="/poisonivy-is-coming-for-you-batman.jpeg" dest_ip="192.168.250.70" | table _time src dest_ip http.hostname url
