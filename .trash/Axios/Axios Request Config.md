
请求配置可传入到 axios 函数中以用于发送一个网络请求。下面是请求配置中可用的配置项，其中仅 `url` 配置项是必须被指定的，当 `method` 配置项没有被指定时则默认发送 GET 请求。

```js
{
	// 给谁发送
	url: '/user',
	
	// 请求类型
	method: 'get', // default
	
	// 设定 URL 的基础结构，如域名和接口地址（真正的URL = baseURL + url）
	baseURL: 'https://some-domain.com/api/',
	
	// 对请求的数据做预处理
	transformRequest: [function (data, headers) {
		// Do whatever you want to transform the data
		return data;
	}],
	
	// 对响应的数据做预处理
	transformResponse: [function (data) {
		// Do whatever you want to transform the data
		return data;
	}],
	
	// 头信息定义，对头信息做控制
	headers: {'X-Requested-With': 'XMLHttpRequest'},
	
	// 指定 URL 的参数（如：https://test.com?a=1&b=2，其中的a=1和b=2即为参数）
	params: {
		ID: 12345,
		a: 1,
		b: 2,
	},
	
	// `paramsSerializer` is an optional config in charge of serializing `params`
	paramsSerializer: {
		indexes: null // array indexes format (null - no brackets, false - empty brackets, true - brackets with indexes)
	},
	
	// 请求体设置，包含对象和字符串两种形式
	data: { firstName: 'Fred' },
	data: 'Country=Brasil&City=Belo Horizonte',
	
	// 请求超时时间
	timeout: 1000, // default is `0` (no timeout)
	
	// 在跨域请求时对 Cookie 的携带设置
	withCredentials: false, // default
	
	// `adapter` allows custom handling of requests which makes testing easier.
	// Return a promise and supply a valid response (see lib/adapters/README.md).
	adapter: function (config) {
		/* ... */
	},
	
	// 请求验证
	auth: {
	username: 'janedoe',
	password: 's00pers3cret'
	},
	
	// 请求格式设置
	responseType: 'json', // default
	
	// 响应结果编码
	responseEncoding: 'utf8', // default
	
	// 跨站请求的 Cookie 标识
	xsrfCookieName: 'XSRF-TOKEN', // default
	
	// 跨站请求的 Header 标识
	xsrfHeaderName: 'X-XSRF-TOKEN', // default
	
	// 上传文件的回调  
	onUploadProgress: function (progressEvent) {
	// Do whatever you want with the native progress event
	},
	
	// 下载文件的回调
	onDownloadProgress: function (progressEvent) {
	// Do whatever you want with the native progress event
	},
	
	// 设置 HTTP 响应体的最大尺寸
	maxContentLength: 2000,
	
	// HTTP 请求体最大尺寸
	maxBodyLength: 2000,
	
	// HTTP 请求成功的判断规则
	// `validateStatus` defines whether to resolve or reject the promise for a given
	// HTTP response status code. If `validateStatus` returns `true` (or is set to `null`
	// or `undefined`), the promise will be resolved; otherwise, the promise will be
	// rejected.
	validateStatus: function (status) {
		return status >= 200 && status < 300; // default
	},
	
	// 最大跳转数
	maxRedirects: 21, // default
	
	// `beforeRedirect` defines a function that will be called before redirect.
	// Use this to adjust the request options upon redirecting,
	// to inspect the latest response headers,
	// or to cancel the request by throwing an error
	// If maxRedirects is set to 0, `beforeRedirect` is not used.
	beforeRedirect: (options, { headers }) => {
		if (options.hostname === "example.com") {
		  options.auth = "user:password";
		}
	},
	
	
	// `socketPath` defines a UNIX Socket to be used in node.js.
	// e.g. '/var/run/docker.sock' to send requests to the docker daemon.
	// Only either `socketPath` or `proxy` can be specified.
	// If both are specified, `socketPath` is used.
	socketPath: null, // default
	
	
	// `httpAgent` and `httpsAgent` define a custom agent to be used when performing http
	// and https requests, respectively, in node.js. This allows options to be added like
	// `keepAlive` that are not enabled by default.
	httpAgent: new http.Agent({ keepAlive: true }),
	httpsAgent: new https.Agent({ keepAlive: true }),
	
	
	// 设置代理
	// `proxy` defines the hostname, port, and protocol of the proxy server.
	// You can also define your proxy using the conventional `http_proxy` and
	// `https_proxy` environment variables. If you are using environment variables
	// for your proxy configuration, you can also define a `no_proxy` environment
	// variable as a comma-separated list of domains that should not be proxied.
	// Use `false` to disable proxies, ignoring environment variables.
	// `auth` indicates that HTTP Basic auth should be used to connect to the proxy, and
	// supplies credentials.
	// This will set an `Proxy-Authorization` header, overwriting any existing
	// `Proxy-Authorization` custom headers you have set using `headers`.
	// If the proxy server uses HTTPS, then you must set the protocol to `https`. 
	proxy: {
		protocol: 'https',
		host: '127.0.0.1',
		port: 9000,
		auth: {
			username: 'mikeymike',
			password: 'rapunz3l'
		}
	},
	
	
	// `cancelToken` specifies a cancel token that can be used to cancel the request
	// (see Cancellation section below for details)
	cancelToken: new CancelToken(function (cancel) {
	}),
	
	
	// an alternative way to cancel Axios requests using AbortController
	signal: new AbortController().signal,
	
	
	// `decompress` indicates whether or not the response body should be decompressed 
	// automatically. If set to `true` will also remove the 'content-encoding' header 
	// from the responses objects of all decompressed responses
	// - Node only (XHR cannot turn off decompression)
	decompress: true // default
	
	
	// `insecureHTTPParser` boolean.
	// Indicates where to use an insecure HTTP parser that accepts invalid HTTP headers.
	// This may allow interoperability with non-conformant HTTP implementations.
	// Using the insecure parser should be avoided.
	// see options https://nodejs.org/dist/latest-v12.x/docs/api/http.html#http_http_request_url_options_callback
	// see also https://nodejs.org/en/blog/vulnerability/february-2020-security-releases/#strict-http-header-parsing-none
	insecureHTTPParser: undefined // default
	
	
	// transitional options for backward compatibility that may be removed in the newer versions
	transitional: {
	// silent JSON parsing mode
	// `true`  - ignore JSON parsing errors and set response.data to null if parsing failed (old behaviour)
	// `false` - throw SyntaxError if JSON parsing failed (Note: responseType must be set to 'json')
	silentJSONParsing: true, // default value for the current Axios version
	
	// try to parse the response string as JSON even if `responseType` is not 'json'
	forcedJSONParsing: true,
	
	// throw ETIMEDOUT error instead of generic ECONNABORTED on request timeouts
	clarifyTimeoutError: false,
	},
	
	
	env: {
	// The FormData class to be used to automatically serialize the payload into a FormData object
		FormData: window?.FormData || global?.FormData
	},
	
	
	formSerializer: {
		visitor: (value, key, path, helpers)=> {}; // custom visitor funaction to serrialize form values
		dots: boolean; // use dots instead of brackets format
		metaTokens: boolean; // keep special endings like {} in parameter key 
		indexes: boolean; // array indexes format null - no brackets, false - empty brackets, true - brackets with indexes
	}
}
```