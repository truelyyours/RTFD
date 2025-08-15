[_Wappalyzer_](https://addons.mozilla.org/en-US/firefox/addon/wappalyzer/) is a browser extension that uncovers the technologies used on websites, such as content management systems (CMS), e-commerce platforms, web frameworks, server software, and analytics tools. It is a valuable tool for reconnaissance, helping identify interesting data like the software components behind a web application or site. Many web technologies, including CMSs and server-side software, leave traces in a website's HTML code, which is publicly accessible for browsers to render. Wappalyzer is highly effective at detecting these technologies through various fingerprinting methods.

_Wappanalyzer_ has already been downloaded on the system. To install it, open a _new terminal_ and type the following command:

`firefox /home/ubuntu-user/Desktop/wappalyzer-6.8.18-fx.xpi &
# Enumerating Wordpress With Wappalyzer

A content management system (CMS) helps create, manage, and modify content on a website. It is a useful tool for building websites, as it allows you to do so without writing all the code from scratch. A CMS is a central tool, and as such, more than half of all websites are built using one.

Open the Firefox browser and visit the following host, which is a website running on WordPress.

`http://172.20.1.114:8000

Click the Wappalyzer icon in the top-right corner of the browser to view additional information about the website's technologies.

Wappalyzer detects WordPress as the CMS and provides details about the site.

This information is useful to attackers for identifying vulnerabilities that could be exploited later.

What is the primary programming language used on this website? **php**
# Enumerating Joomla With Wappalyzer

Open a new Firefox tab and visit _joomla Server_.
`http://172.20.1.114:8080

Enumerate the technologies in use with Wappalyzer by clicking on the extension.

Joomla is another open-source content management system. It is mainly used to build powerful dynamic websites and applications. Joomla has gained massive popularity over the last decade and has successfully grown into one of the most widely used content management systems globally. In this scenario, Wappalyzer can identify Joomla within seconds of visiting the website. Additionally, Wappalyzer detects Bootstrap as a UI framework used on the website followed by the operating system and used programming languages.

### Enumerating Drupal With Wappalyzer

Open a new Firefox tab and visit _drupal Server_.
`http://172.20.1.115:8080

Click the Wappalyzer extension in order to display the used technologies.

Drupal is another web content management tool as well as a customizable platform. Wappalyzer is quick to detect its presence on the site while also managing to display its version number. Let's assume that the specific version has a vulnerability. If an attacker were to know this information they could use it to exploit the vulnerability and cause significant damage to the victim.

What version of Drupal is being used on the website? **8**
# Introduction To WhatWeb

[_WhatWeb_](https://github.com/urbanadventurer/WhatWeb) is another tool used for website enumeration. It's a CLI tool that detects web technologies using over 1700 plugins, each designed for a different purpose. WhatWeb identifies version numbers, email addresses, account IDs, SQL errors, and more. It can operate stealthily and quickly or thoroughly but slowly, with an aggression level to balance speed and accuracy.

When visiting a website, many hints about its underlying technologies are visible. Sometimes, a single page provides enough information to identify the site. If not, WhatWeb performs a deeper scan. The default "stealthy" mode is the fastest and requires just one HTTP request, ideal for public websites. Higher aggression levels are more thorough and suited for penetration testing.

Most WhatWeb plugins are capable of detecting both subtle and obvious clues. For example, WordPress sites are often identified by a meta HTML tag, but if removed, WhatWeb still uses other methods like checking for default files, the favicon, and "/wp-content/" in links.

To list available commands, first navigate to whatweb directory and execute the following commands:

cd whatweb

./whatweb --help

### Enumerating WordPress With WhatWeb

Use the following command to enumerate WordPress through WhatWeb:

./whatweb 172.20.1.114:8000

WhatWeb manages to retrieve information such as the Websites title, the Apache version number, the WordPress version number, the IP Address, and the Operating System.

---

What is the version of WordPress displayed in the output?