https://www.cnblogs.com/wanghui-garcia/p/10455217.html

% dlv debug exp1/main.go 
could not launch process: stub exited while waiting for connection: exit status 0

vscode 的terminal 配置不正确
 % dlv debug exp1/main.go
Type 'help' for list of commands.
(dlv)

(dlv) c
Starting main
count :  0
count :  1
count :  2
count :  3
count :  4

(dlv) b main.main
Process 64256 has exited with status 0
(dlv) r
Process restarted with PID 64317
(dlv) b main.main
Breakpoint 1 set at 0x1008d4120 for main.main() ./exp1/main.go:16
(dlv) c
> main.main() ./exp1/main.go:16 (hits goroutine(1):1 total:1) (PC: 0x1008d4120)
    11:			c <- i
    12:		}
    13:		close(c)
    14:	}
    15:
=>  16:	func main() {
    17:		msg := "Starting main"
    18:		fmt.Println(msg)
    19:		bus := make(chan int)
    20:		msg = "starting a gofunc"
    21:		go counting(bus)
(dlv) bp
Breakpoint runtime-fatal-throw at 0x100853440 for runtime.fatalthrow() /usr/local/go/src/runtime/panic.go:1163 (0)
Breakpoint unrecovered-panic at 0x1008534a0 for runtime.fatalpanic() /usr/local/go/src/runtime/panic.go:1190 (0)
	print runtime.curg._panic.arg
Breakpoint 1 at 0x1008d4120 for main.main() ./exp1/main.go:16 (1)
(dlv)


(dlv) p msg
"starting a gofunc"
(dlv) locals
msg = "starting a gofunc"
bus = chan int 0/0
(dlv) p msg == "Starting main"
false
(dlv) whatis msg
string
(dlv) help goroutine
Shows or changes current goroutine

	goroutine
	goroutine <id>
	goroutine <id> <command>

Called without arguments it will show information about the current goroutine.
Called with a single argument it will switch to the specified goroutine.
Called with more arguments it will execute a command on the specified goroutine.
(dlv)  goroutine 1
Switched from 1 to 1 (thread 3196597)
(dlv) locals
msg = "starting a gofunc"
bus = chan int 0/0
(dlv)  goroutine 18
Switched from 1 to 18 (thread 3196597)
(dlv) locals
msg = "starting a gofunc"
bus = chan int 0/0
(dlv) args
(no args)
(dlv) help on
Executes a command when a breakpoint is hit.

	on <breakpoint name or id> <command>.

Supported command



https://www.cnblogs.com/li-peng/p/8522592.html


%  dlv debug exp2/main.go
Type 'help' for list of commands.
(dlv) b main.main
Breakpoint 1 set at 0x102fa4770 for main.main() ./exp2/main.go:12
(dlv) b main.hi
Breakpoint 2 set at 0x102fa48f0 for main.hi() ./exp2/main.go:19
(dlv) c
> main.main() ./exp2/main.go:12 (hits goroutine(1):1 total:1) (PC: 0x102fa4770)
     7:		"os"
     8:	)
     9:
    10:	const port = "8000"
    11:
=>  12:	func main() {
    13:		http.HandleFunc("/hi", hi)
    14:
    15:		fmt.Println("runing on port: " + port)
    16:		log.Fatal(http.ListenAndServe(":"+port, nil))
    17:	}
(dlv) c
runing on port: 8000
> main.hi() ./exp2/main.go:19 (hits goroutine(20):1 total:1) (PC: 0x102fa48f0)
    14:
    15:		fmt.Println("runing on port: " + port)
    16:		log.Fatal(http.ListenAndServe(":"+port, nil))
    17:	}
    18:
=>  19:	func hi(w http.ResponseWriter, r *http.Request) {
    20:		hostName, _ := os.Hostname()
    21:		fmt.Fprintf(w, "HostName: %s", hostName)
    22:	}

% curl localhost:8000/hi

(dlv) p r
*net/http.Request {
	Method: "GET",
	URL: *net/url.URL {
		Scheme: "",
		Opaque: "",
		User: *net/url.Userinfo nil,
		Host: "",
		Path: "/hi",
		RawPath: "",
		ForceQuery: false,
		RawQuery: "",
		Fragment: "",
		RawFragment: "",},
	Proto: "HTTP/1.1",
	ProtoMajor: 1,
	ProtoMinor: 1,
	Header: net/http.Header [
		"User-Agent": [
			"curl/7.64.1",
		],
		"Accept": ["*/*"],
	],
	Body: io.ReadCloser(net/http.noBody) {},
	GetBody: nil,
	ContentLength: 0,
	TransferEncoding: []string len: 0, cap: 0, nil,
	Close: false,
	Host: "localhost:8000",
	Form: net/url.Values nil,
	PostForm: net/url.Values nil,
	MultipartForm: *mime/multipart.Form nil,
	Trailer: net/http.Header nil,
	RemoteAddr: "[::1]:64372",
	RequestURI: "/hi",
	TLS: *crypto/tls.ConnectionState nil,
	Cancel: <-chan struct {} {},
	Response: *net/http.Response nil,
	ctx: context.Context(*context.cancelCtx) *{
		Context: context.Context(*context.cancelCtx) ...,
		mu: (*sync.Mutex)(0x14000090150),
		done: chan struct {} {},
		children: map[context.canceler]struct {} nil,
		err: error nil,},}
(dlv) c

https://www.thegreatcodeadventure.com/debugging-a-go-web-app-with-vscode-and-delve/


https://segmentfault.com/a/1190000018671207

http://llever.com/pass-blog/2018/10/29/%E8%AF%91%E5%A6%82%E4%BD%95%E5%9C%A8vscode%E4%B8%AD%E4%BD%BF%E7%94%A8delve%E8%B0%83%E8%AF%95%E4%BB%A3%E7%A0%81/


https://github.com/go-delve/delve/issues/1534
sudo /usr/sbin/DevToolsSecurity --enable


exec /usr/bin/arch -arch arm64 /Users/eaglemoor/go/bin/dlv "$@"

https://github.com/go-delve/delve/issues/2332

https://stackoverflow.com/questions/65456912/goland-on-apple-silicon-can-not-debug-golang-project


https://github.com/golang/vscode-go/blob/master/docs/debugging.md

