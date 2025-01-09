# Run-FFuF-On-Multiple-URLs
Fuzz multiple URLs at once using FFuF

## Method-1 Fuzzing URLs one by one: 

The below one-liner command will pick the URLs from the urls.txt file one by one and fuzz the content.

```
while read -r url; do ffuf -u "$url/FUZZ" -w wordlist.txt -o "$(echo "$url" | sed 's~http[s]*://~~' | tr '/:' '_').json"; done < urls.txt

```

**Output Files:**
example_com.json: Contains the fuzzing results for http://example.com.
test_com_api.json: Contains the fuzzing results for https://test.com/api.
localhost_8080.json: Contains the fuzzing results for http://localhost:8080.
192_168_1_1_admin.json: Contains the fuzzing results for https://192.168.1.1/admin.
Notes:

## Method-2 Fuzzing the URLs concurrently:

The below one-liner command will take multiple urls at once (number of urls to be taken can be controlled using -P flag)

```
cat urls.txt | xargs -P 10 -I {} ffuf -u {}/FUZZ -w wordlist.txt -o "$(echo {} | sed 's~http[s]*://~~' | tr '/:' '_').json"
```


**Explanation:**
1. cat urls.txt: Reads the URLs from the file urls.txt.
2. xargs: Runs the ffuf command for each URL in parallel.
3. -P 10: Specifies 10 parallel jobs (you can adjust this based on your system resources).
4. -I {}: Replaces {} with each URL from urls.txt.
5. ffuf: Fuzzes each URL using the specified wordlist.
Output filenames: The filenames are sanitized using sed and tr to replace special characters.

**Output Files:**
example_com.json
test_com_api.json
localhost_8080.json
192_168_1_1_admin.json
