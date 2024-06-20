Ready to dive into the world of Go without getting tangled in the web? 🕸️🐐 In this tutorial, we’re not just building any web server; we’re crafting a Go-powered digital butler that caters to your static files and juggles HTTP requests like a pro. By the end of our adventure, you’ll be the one telling Go jokes! Let’s code and chuckle our way through this, shall we?

Prerequisite

Install Go: Ensure Go is installed on your system.

(Download and install - The Go Programming Language)

Basic understanding of Go programming concept

I learned from here.

((236) Golang Tutorials For Beginners in depth - YouTube)-

and also you can follow this comprehensive guide. repo 👇

ChetanThapliyal/get-started-with-Go: Quick Introduction to start with Go. (github.com) You can also follow this repo for a comprehensive guide.

Getting Started

Let's start by creating a simple web server that serves static HTML files and handles HTTP requests. Follow these steps:

Step 1: Setting up your workspace

Create a new directory in your desired location

mkdir Go-webserver
cd Go-webserver

Inside the project directory create a subdir named static

(static: -your html and CSS files will be stored inside it)

mkdir static
cd static

Create two HTML files named index.html and form.html inside the static directory. You can use any text editor to create these files and add basic HTML content.

touch index.html form.html

Visit this to populate the above files (copy the code from the link mentioned below and paste in the above files):

index.html: - Get-Set-Go/Web Server with Go/static/index.html at main · itsBaivab/Get-Set-Go (github.com)

form.html: - Get-Set-Go/Web Server with Go/static/form.html at main · itsBaivab/Get-Set-Go (github.com)

Use tree command to check the directory structure

Go-server/
 └── static
     ├── form.html
     └── index.html

Navigate to the Go-server root folder make a file name main.go

touch main.go

Now create a GitHub repo where you will upload this code

I am using this repository for maintaining the project

(note: - if you don't know how to create a GitHub repository open a GitHub account follow this tutorial tutorial link(Youtube) 🖇️ )

Now copy URL of your repository without the https:// text. Your copied text should look like this github.com/<your-user-name>/<your-repo-name>. and then run the below code.

(note:- replace the angular brackets also while copy pasting your credentials )

go mod init  github.com/<your-user-name>/<your-repo-name>

Step 2: - Writing Go code

Open main.go file and copy paste the below code

package main

import (
	"fmt"
	"log"
	"net/http"
)

func formHandler(w http.ResponseWriter, r *http.Request) {
	if err := r.ParseForm(); err != nil {
		fmt.Fprintf(w, "ParseForm() err: %v", err)
		return
	}
	fmt.Fprintf(w, "POST request successful\n")
	name := r.FormValue("name")
	email := r.FormValue("email")
	message := r.FormValue("message")
	fmt.Fprintf(w, "Name = %s\n", name)
	fmt.Fprintf(w, "Email = %s\n", email)
	fmt.Fprintf(w, "Message = %s\n", message)
}
func helloHandler(w http.ResponseWriter, r *http.Request) {
	if r.URL.Path != "/hello" {
		http.Error(w, "404 not found", http.StatusNotFound)
		return
	}
	if r.Method != "GET" {
		http.Error(w, "Method not allowed", http.StatusMethodNotAllowed)
		return
	}
	fmt.Fprintf(w, "Hello, World!")
}

func main() {
	fileserver := http.FileServer(http.Dir("./static")) // autometically pics up the index.html file
	http.Handle("/", fileserver)
	http.Handle("/form", http.HandlerFunc(formHandler))
	http.Handle("/hello",http.HandlerFunc( helloHandler))

	fmt.Println("Server is running on port 8080\n")
	if err := http.ListenAndServe(":8080", nil); err != nil {
		log.Fatal(err)
	}// nil is the default server mux // this will create the server

}

Step 3: -

Open a terminal window and navigate to your project directory.

Run the following command to build and run the web server:

 go build 
 go run main.go

Open your web browser and visit http://localhost:8080to see your web server in action. You should be able to see the following:

localhost:8080

localhost:8080/hello

localhost:8080/form.html

(note: - I populated this it will be empty in your side)

after clicking on the submit button

Brief overview of the main.go file

Let's break down the components one by one

Importing Packages: We import necessary packages like

fmt: Package for formatted I/O (used for printing messages).

log: Package for logging.

net/http Package for building HTTP servers & for handling HTTP requests and responses.

Let’s define our dynamic duo of handler functions:

helloHandler: This vigilant function patrols the “/hello” route, ensuring only “GET” requests pass through. Stray paths receive a stern 404, but rightful visitors are greeted with a warm “Hello there”.

formHandler: The “/form” route’s gatekeeper, this function only entertains “POST” requests. It meticulously parses form data, retrieves “name” and “address” with finesse, and celebrates successful submissions with a personalized success message.

Serving Static Files: We create a file server to serve static files from the static directory using http.FileServer. The "/" route refers to the fileserver as a home route and by default it returns index.html file present in the static folder

Route Handling: We use http.HandleFunc to associate our handler functions with specific routes, such as /hello and /form.

Listening and Serving: We use http.ListenAndServe to start the web server on port 8080. If an error occurs, we log it.

Going deeper in the functions

HelloHandler: - The helloHandler part of the code is responsible for responding to HTTP requests that target the “/hello” path. Here’s a summary of its functionality:

Path Check: It first checks if the requested URL path is exactly “/hello”. If not, it sends a 404 not found error.

Method Check: It then checks if the request method is “GET”. If not, it sends a Method not allowed error.

Response: If both checks pass, it responds with the message “Hello, World!”.

This handler ensures that only GET requests to the “/hello” path are served, maintaining strict routing and method compliance.

FormHandler: - The formHandler function serves as a processor for POST requests sent to the “/form” endpoint. Here’s a breakdown of its tasks:

Form Parsing: It begins by parsing the form data included in the request. If there’s an error during parsing, it responds with an error message detailing the issue.

Data Extraction: After successful parsing, it extracts the values for “name,” “email,” and “message” fields from the form data.

Success Response: It then sends a confirmation message back to the client, including the extracted data, indicating that the POST request was successfully processed.

This handler is crucial for handling form submissions and extracting necessary information from POST requests.

main() :- Here’s a line-by-line breakdown of the main function:

fileserver := http.FileServer(http.Dir("./static")): Creates a new file server that serves static files from the “static” directory located in the same directory as the Go program.

http.Handle("/", fileserver): Registers the file server as the handler for the root URL path. This means any request to the root path will be served by the file server, which will look for a corresponding file in the “static” directory.

http.Handle("/form", http.HandlerFunc(formHandler)): Registers the formHandler function as the handler for the “/form” URL path. This means any request to “/form” will be processed by formHandler.

http.Handle("/hello", http.HandlerFunc(helloHandler)): Registers the helloHandler function as the handler for the “/hello” URL path. This means any request to “/hello” will be processed by helloHandler.

fmt.Println("Server is running on port 8080\n"): Prints a message to the console indicating that the server is starting up and will listen on port 8080.

if err := http.ListenAndServe(":8080", nil); err != nil {: Starts an HTTP server listening on port 8080. If there’s an error during startup, it will be captured.

log.Fatal(err): If an error occurred while starting the server, this line logs the error and exits the program.

The main function is critical for configuring and starting up the web server, defining how it handles incoming requests, and serving static content.

Usages

When this program is executed, it starts an HTTP server on port 8080. Here's what it does for different routes:

Visiting the root ("/") route serves static files from the "./static" directory. By default, It servers the index.html file.

Accessing the "/form" route displays a simple HTML form. Upon submitting the form, it processes the data and displays the "name", "address", "Email" and "Message" values.

Accessing the "/hello" route responds with "Hello world".

Conclusion

And there you have it—a sleek, efficient web server in Go, ready to serve with speed and precision. From the vigilant helloHandler to the meticulous formHandler, each line of code weaves together to form a robust digital tapestry. Whether it’s greeting visitors with a friendly “Hello, World!” or handling form submissions with grace, this server stands as a testament to the power of Go’s simplicity and performance. So deploy with confidence and watch your server come alive, humming on port 8080, eager to connect and serve in the vast expanse of the web. 🚀✨
