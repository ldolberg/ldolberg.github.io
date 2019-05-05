---
title: "Host Probing with NodeMCU"
date: 2019-04-18T15:34:30-04:00
categories:
  - blog
tags:
  - ESP8266
  - networks
  - IoT
---
Not so long ago, I got an nodemcu [esp8266](https://www.nodemcu.com/index_en.html). This device is awesome for prototyping and developing IoT gadgets.

Since some devices might go on and off from my network, and I wanted to start with something simple to find out who is in. 

This dead simple host scanner does the job pretty well. The idea behing is to loop a local network address space while probing if hosts are responding for ping. 

The nodemcu can be programmed in either LUA or C++, I chose the latter because I wanted to get used to Arduino IDE. 

In this case we are just going to need the ESP8266WiFi and ESP8266Ping libraries.


```cpp
//#include <SPI.h>
#include <ESP8266WiFi.h>
#include <ESP8266Ping.h>

const char* ssid = "**";
const char* password = "**";
int i = 1;

 
void setup() {
  Serial.begin(115200);
  // Connect to WiFi network
  Serial.println();
  Serial.println();
  Serial.print("Connecting to ");
  Serial.println(ssid);
 
  WiFi.begin(ssid, password);
 
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("");
  Serial.println("WiFi connected");
}


void loop() {
  Serial.print("Pinging ");
  Serial.print(": ");
  IPAddress ip (192, 168, 178, i); // The remote ip to ping
  Serial.print(ip);
  bool pingResult = Ping.ping(ip, 2);
  if (pingResult) {
    Serial.print("SUCCESS! RTT = ");
    Serial.print(pingResult);
    Serial.println(" ms");
  } else {
    Serial.print("FAILED! Error code: ");
    Serial.println(pingResult);
  }
  i = i + 1;
```

It will go something like 
```
19:27:12.040 -> Pinging : 192.168.178.1SUCCESS! RTT = 1 ms
19:27:13.089 -> Pinging : 192.168.178.2FAILED! Error code: 0
19:27:15.128 -> Pinging : 192.168.178.3FAILED! Error code: 0
19:27:17.192 -> Pinging : 192.168.178.4FAILED! Error code: 0
19:27:19.196 -> Pinging : 192.168.178.5FAILED! Error code: 0
```
<!-- You'll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

To add new posts, simply add a file in the `_posts` directory that follows the convention `YYYY-MM-DD-name-of-post.ext` and includes the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Jekyll also offers powerful support for code snippets:

```ruby
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
```

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/ -->
