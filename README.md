[![Build Status](https://travis-ci.org/nerdalert/gopher-net-ctl.svg?branch=master)](https://travis-ci.org/nerdalert/gopher-net-ctl)

# gopher-net-cli
CLI for Gopher Net Router

### Example Advertise a new route

-Example adding a prefix to advertise into the tables (only a couple of fields are plumbed in until it uses the daemon data structures):

	gopher-net-ctl -d add route --prefix=172.15.14.0/24  --nexthop=172.16.86.1

-Example Output in Quagga

	ub134(config-router)# do sho ip route
	Codes: K - kernel route, C - connected, S - static, R - RIP,
	       O - OSPF, I - IS-IS, B - BGP, A - Babel,
	       > - selected route, * - FIB route

	K>* 0.0.0.0/0 via 172.16.86.2, eth0
	C>* 127.0.0.0/8 is directly connected, lo
	B>* 172.15.14.0/24 [200/0] via 172.16.86.1, eth0, 00:00:03
	C>* 172.16.86.0/24 is directly connected, eth0

### Example remove the advertised route

-This will stop advertising the requested prefix. There is some cidr from the specified node.from the specified note a new route

	gopher-net-ctl -d delete route --prefix=172.15.14.0/24

Example Output in Quagga

	ub134(config-router)# do sho ip route
	Codes: K - kernel route, C - connected, S - static, R - RIP,
	       O - OSPF, I - IS-IS, B - BGP, A - Babel,
	       > - selected route, * - FIB route

	K>* 0.0.0.0/0 via 172.16.86.2, eth0
	C>* 127.0.0.0/8 is directly connected, lo
	C>* 172.16.86.0/24 is directly connected, eth0

### Host Route Support

This and the API will also support hostnames that are resolvable by the target node/bgp speaker.

-Add a hosts DNS name (or VIP) to be advertised.

	gopher-net-ctl -d add route --prefix=google-public-dns-a.google.com  --nexthop=172.16.86.1

-The the resolved address will be advertised into the IGP/EGP as in the Quagga interface output below that is peered to an instance of the gnet daemon.

	ub134(config-router)# do sho ip route
	Codes: K - kernel route, C - connected, S - static, R - RIP,
	       O - OSPF, I - IS-IS, B - BGP, A - Babel,
	       > - selected route, * - FIB route

	K>* 0.0.0.0/0 via 172.16.86.2, eth0
	B>* 8.8.8.8/32 [200/0] via 172.16.86.1, eth0, 00:00:08
	C>* 127.0.0.0/8 is directly connected, lo
	B>* 172.15.14.0/24 [200/0] via 172.16.86.1, eth0, 00:07:46
	C>* 172.16.86.0/24 is directly connected, eth0

## Contributing

Jump in and join us!

### Testing

Simple run `make` prior to a PR. Test files are slim at the moment.

### Install godep

We use godep for dependency management and versioning. Please see the project page for more details about [godep](https://github.com/tools/godep)

    $ go get github.com/tools/godep

The following should drop you into the Godep project directory. If it doesn't there is a GOPATH issue.

    $ cd $GOPATH/src/github.com/tools/godep
    $ go build ./...
    $ go install ./...

### Verify Godep install

Now you should be able to run godep and it should be in your $PATH from you Go environment.

    $ godep
    Godep is a tool for managing Go package dependencies.

    Usage:

         godep command [arguments]

    The commands are:

        save     list and copy dependencies into Godeps
        go       run the go tool with saved dependencies
        get      download and install packages with specified dependencies
        path     print GOPATH for dependency code
        restore  check out listed dependency versions in GOPATH
        update   use different revision of selected packages

    Use "godep help [command]" for more information about a command.


### Using Godep

Import should be normal to begin with:

`import "github.com/codegangsta/cli"`

    $ go build ./...

then running godep save will create the .Godep directory in the project.

    $ godep save -r ./...

Now your imports should look like this:

`"github.com/gopher-net/gnet-ctl/Godeps/_workspace/src/github.com/codegangsta/cli"`
