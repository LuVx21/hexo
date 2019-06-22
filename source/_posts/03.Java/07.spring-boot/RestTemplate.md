

```Java
HttpHeaders headers = new HttpHeaders();
List<String> cookies = new ArrayList<>();
cookies.add("JSESSIONID=" + Strings.nullToEmpty(jsessionId));
cookies.add("token=" + Strings.nullToEmpty(token));
headers.put(HttpHeaders.COOKIE,cookies);
HttpEntity request = new HttpEntity(null, headers);
ResponseEntity<String> response = restTemplate.postForEntity(url, request, String.class);
```


post表单
```Java
HttpHeaders headers = new HttpHeaders();
headers.setContentType(MediaType.APPLICATION_FORM_URLENCODED);
MultiValueMap<String, String> map = new LinkedMultiValueMap<String, String>();
map.add("title", title);
map.add("desc", desc);
map.add("userid", toUserId);
HttpEntity<MultiValueMap<String, String>> request = new HttpEntity<MultiValueMap<String, String>>(map, headers);
ResponseEntity<String> response = restTemplate.postForEntity(url, request, String.class);
```


post json
```Java
HttpHeaders headers = new HttpHeaders();
headers.setContentType(MediaType.APPLICATION_JSON);
headers.setAccept(Lists.newArrayList(MediaType.APPLICATION_JSON));
HttpEntity<String> entity = new HttpEntity<String>(requestJson, headers);
ResponseEntity<String> resp = restTemplate.postForEntity(url,entity,String.class);
```