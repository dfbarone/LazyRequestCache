# CachingOkHttp
A wrapper around okhttp to help persist HTTP GET responses by using the okhttp disk cache.

Main purpose to use this is to 
1) Enforce the use of max-age (via network interceptor) for (static URL) HTTP GET calls to limit network use.
2) Use http disk cache when offline (via interceptor).
3) Optionally implement CachingInterface to persist all HTTP responses.

```groovy
    // Initialize a CachingOkHttpClient
    CachingOkHttpClient httpClient = new CachingOkHttpClient.Builder(context/*to check network availability*/)
                .okHttpClient(new OkHttpClient.Builder().build())
                .maxAge(5*60/*5 minutes default*/)
                .maxStale(60*60*24/*1 day default*/)
                .cache("cache_name", SIZE_IN_MB)
                .dataStore(new CachingInterfaceImpl())
                .build();
```

```groovy
    // Build a CachingRequest
    CachingRequest request = new CachingRequest.Builder(
                new okhttp3.Request.Builder()
                    .url(url)
                    .get()
                    .addHeader(..., ...)
                    .build())
                .maxAge(60/*1 minute for this request*/)
                .build();
```

```groovy
    // Fetch your data as a string
    Response response = httpClient.get(request, Response.class);
    
    // Fetch your data as a string
    httpClient.getAsync(request, String.class)
        .subscribeOn(Schedulers.io())
        .observeOn(AndroidSchedulers.mainThread())
        .subscribe();
```

License
-------
    Copyright 2017 Dominic F. Barone

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.

