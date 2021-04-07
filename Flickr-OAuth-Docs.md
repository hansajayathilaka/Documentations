# Flickr OAuth Flow

## Login to Flickr & Create Flickr App

+ Create Flickr Account - <https://identity.flickr.com/sign-up>
+ Login to your Flick - <https://identity.flickr.com/>
+ Create Flickr App - <https://www.flickr.com/services/apps/create/>
+ Click on ***Request API Key***
+ Click on ***Apply For A Non Commercial API Key***
+ Note down API Key (Consumer Key) and API Secret (Consumer Secret) for your app

Notes:

+ This page show how flickr oauth works - <https://www.flickr.com/services/api/auth.oauth.html>
+ Video tutorial about flickr oauth - <https://www.youtube.com/watch?v=3gXPjj5iEAA>

<br>
<br>

## 1. Getting Request Token

Follow steps in this page :<br>
<http://www.wackylabs.net/2011/12/oauth-and-flickr-part-2/>

**Request Token URL** - <https://www.flickr.com/services/oauth/request_token>

**Request Token URL Parameters :**

+ `oauth_nonce` - Random string, unique for each request. This allows the Service Provider to verify that a request has never been made before and helps prevent replay attacks when requests are made over a non-secure channel.<br>

```TypeScript
generateNonce(): string {
    const charset: string =
      '0123456789ABCDEFGHIJKLMNOPQRSTUVXYZabcdefghijklmnopqrstuvwxyz';
    const result: string[] = [];
    window.crypto
      .getRandomValues(new Uint8Array(32))
      .forEach((c) => result.push(charset[c % charset.length]));
    return result.join('');
  }
```

<br>

+ `oauth_timestamp` - The timestamp value must be a positive integer and must be equal or greater than the timestamp used in previous requests

```TypeScript
const timestamp = (+ new Date()).toString()
```

<br>

+ `oauth_consumer_key` - API Key for your Flickr App

<br>

+ `oauth_signature_method` - Signature Algorithm. The HMAC-SHA1 signature method uses the HMAC-SHA1 signature algorithm where the Signature Base String is the text and the key is the concatenated values of the Consumer Secret and Token Secret, separated by an '&' character even if empty. **Currently, Flickr only supports HMAC-SHA1 signature encryption**

<br>

+ `oauth_version` - OAuth Version. This parameter is an optional parameter and it is not required

<br>

+ `oauth_callback` - Callback URL

+ `oauth_signature` - You can create this with steps in [this](http://www.wackylabs.net/2011/12/oauth-and-flickr-part-2/) page

<br>
<br>

### **Create OAuth Signature**

First, you need to get your oauth parameters set. These are dummy data

+ Timestamp= “1316657628”
+ Nonce = “C2F26CD5C075BA9050AD8EE90644CF29”
+ Consumer Key = “768fe946d252b119746fda82e1599980”
+ Version = “1.0”
+ Signature Method =  “HMAC-SHA1”
+ Callback URL = “http://www.wackylabs.net/oauth/test”

Consumer Secret = “1a3c208e172d3edc”

To generate a SHA1 signature you require two things

1. Key
2. Base String

**Key = Consumer Secret + “&” + Token Secret**<br>
As this is the first stage you have neither, so your token secret is simply an empty string. Your key looks like `"1a3c208e172d3edc&”`

**Base String = Request Method + "&" + Request URL + "&" + URL Parameters**<br>
URL parameters should be **percent encoded** and must be sorted in alphabetical order. <br>You can use `encodeURIComponent()` function for percent encode. <br>Join parameters with '&' character.
<br>
<br>Your parameter string should be like this :<br>
`oauth_callback=http%3A%2F%2Fwww.wackylabs.net%2Foauth%2Ftest&oauth_consumer_key=768fe946d252b119746fda82e1599980&oauth_nonce=C2F26CD5C075BA9050AD8EE90644CF29&oauth_signature_method=HMAC-SHA1&oauth_timestamp=1316657628&oauth_version=1.0`

And then you encode parameter string again using `encodeURIComponent()`
<br>It looks like this :<br>
`oauth_callback%3Dhttp%253A%252F%252Fwww.wackylabs.net%252Foauth%252Ftest%26oauth_consumer_key%3D768fe946d252b119746fda82e1599980%26oauth_nonce%3DC2F26CD5C075BA9050AD8EE90644CF29%26oauth_signature_method%3DHMAC-SHA1%26oauth_timestamp%3D1316657628%26oauth_version%3D1.0`

After that create base string.<br>
**Base String = Request Method + "&" + Request URL + "&" + URL Parameters**<br>

It looks like this :<br>
`GET&http%3A%2F%2Fwww.flickr.com%2Fservices%2Foauth%2Frequest_token&oauth_callback%3Dhttp%253A%252F%252Fwww.wackylabs.net%252Foauth%252Ftest%26oauth_consumer_key%3D768fe946d252b119746fda82e1599980%26oauth_nonce%3DC2F26CD5C075BA9050AD8EE90644CF29%26oauth_signature_method%3DHMAC-SHA1%26oauth_timestamp%3D1316657628%26oauth_version%3D1.0`

<br>

Now Create OAuth Signature using `Crypto-js` module :

```TypeScript
import * as CryptoJS from 'crypto-js';
const oauth_signature: string = CryptoJS.HmacSHA1(
      base_string,
      key
    ).toString(CryptoJS.enc.Base64);
```

`oauth_signature` looks like this : ` “0fhNGlzpFNAsTme/hDfUb5HPB5U=”`

<br>

### **Requesting the Request Token**

Now you have your signature you can create the url to actually request your request token with.

Note, when generating your URL you must again encode everything (including the new oauth_signature parameter) but you do not need to use the strict OAuth encoding, but can return to normal url encoding.

URL = `http://www.flickr.com/services/oauth/request_token?oauth_callback=http%3A%2F%2Fwww.wackylabs.net%2Foauth%2Ftest&oauth_consumer_key=768fe946d252b119746fda82e1599980&oauth_nonce=C2F26CD5C075BA9050AD8EE90644CF29&oauth_timestamp=1316657628&oauth_signature_method=HMAC-SHA1&oauth_version=1.0&oauth_signature=0fhNGlzpFNAsTme%2FhDfUb5HPB5U%3D`

Make a GET request to this url. If it succeeded, it will gives us Request Token and Token Secret