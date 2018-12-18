# Securing the Client

One big philosophy for Solid++ is data should be inherently secure in your own ecosystem. In most cases, using a third paty application should not require the user to consent to sharing data with that third party. However, this presents a serious flaw if a third party client is able to make requests to a remote third-party server. Because of this, Solid++ requests that easy to install security provisions are put on a user's personal device.

These security provisions are designed to block requests from an application or web-page that are to unapproved servers. Before an application can read or write data from a pod, it must first authorize itself with the pod. This request will return a header `x-allowed-domains` which dictates the domains only domains this app can make requests to. Once this header is returned, a security add-on ensures that the application/webpage cannot make any requests outside of the whitelisted domains.

## Securing Web Browsers

Most browser extensions support intercepting network traffic. Once a website recieves a token and `x-allowed-domains` header from the pod, all subsequent traffic from that page is locked down. An implementation for Chrome has already been made:

https://github.com/o-development/pod-chrome-extension

## Securing Mobile Devices

Mobile devices can be secured using the same technique as the browser extension, but this time using the devices VPN api to monitor network traffic. This technique has been used by ad blockers:

Android: https://github.com/julian-klode/dns66

iOS: https://itunes.apple.com/us/app/adblock/id691121579?mt=8