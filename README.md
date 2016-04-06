Uber
=======

> Golang bindings for [Uber API](https://developer.uber.com/docs/api-overview)

[![Build Status](https://travis-ci.org/chrisenytc/uber.png)](https://travis-ci.org/chrisenytc/uber)
[![GoDoc](https://godoc.org/github.com/chrisenytc/uber?status.svg)](https://godoc.org/github.com/chrisenytc/uber)

## Getting Started

1º Install twilio

```bash
$ go get github.com/chrisenytc/uber
```

# Documentation

For more information about using this library, view the [Godocs](http://godoc.org/github.com/chrisenytc/go-uber).

# Usage

## Register Your Application

In order to use the Uber API you must register an application at the [Uber Developer Portal](https://developer.uber.com).
In turn you will receive a `client_id`, `secret`, and `server_token`.

## Creating a Client

```go
package main

import (
  uber "github.com/chrisenytc/uber"
)

func main() {
	client := uber.NewClient(SERVER_TOKEN)
}
```

## Making Requests

Currently, the Uber API offers support for requesting information about products (e.g available cars), price estimates, time estimates, user ride history, and user info. All requests require a valid `server_token`. Requests that require latitude or longitude as arguments are float64's (and should be valid lat/lon's).

```go
products, err := client.GetProducts(37.7759792, -122.41823)
if err != nil {
	fmt.Println(err)
} else {
	for _, product := range products {
		fmt.Println(*product)
	}
}

prices, err := client.GetPrices(41.827896, -71.393034, 41.826025, -71.406892)
if err != nil {
	fmt.Println(err)
} else {
	for _, price := range prices {
		fmt.Println(*price)
	}
}
```
## Authorizing

Uber's OAuth 2.0 flow requires the user go to URL they provide.

You can generate this URL programatically by doing:


```go
url, _ := client.OAuth(
	CLIENT_ID, CLIENT_SECRET, REDIRECT_URL, "profile",
)
```

After the user goes to `url` and grants your application permissions, you need to figure out a way for the user to input the second argument of the url to which they are redirected (ie: `REDIRECT_URL/?state=go-uber&code=AUTH_CODE`). You then need to

```go
client.SetAccessToken(AUTH_CODE)
```

Or you can automate the whole process by:

```go
err := client.AutOAuth(
	CLIENT_ID, CLIENT_SECRET, REDIRECT_URL, "profile",
)
```

At which point, feel free to

```go
profile, err := client.GetUserProfile()
if err != nil {
	fmt.Println(err)
} else {
	fmt.Println(profile)
}
```

## Contributing

Bug reports and pull requests are welcome on GitHub at [https://github.com/chrisenytc/uber](https://github.com/chrisenytc/uber). This project is intended to be a safe, welcoming space for collaboration, and contributors are expected to adhere to the [Contributor Covenant](http://contributor-covenant.org) code of conduct.

1. Fork it [chrisenytc/uber](https://github.com/chrisenytc/uber/fork)
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am "Add some feature"`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request

## Support
If you have any problem or suggestion please open an issue [here](https://github.com/chrisenytc/uber/issues).

## Credits
This is a fork of the project [r-medina/go-uber](https://github.com/r-medina/go-uber). Give some respect to him.

## License 

Check [here](LICENSE).
