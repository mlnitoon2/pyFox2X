# pyFox2X - implementation of the SFS2X protocol in Python

pyFox2X is a Python library that provides an implementation of the SFS2X (SmartFoxServer 2X) protocol for working with objects and data arrays. The library allows you to create and manipulate objects and arrays, obtain data from them, it also implements compilation and decompilation of packages and functions for communicating with servers.

## Examples of using

### 1.  Working with objects

```python
from pyfox2x.sfs_types.SFSObject import SFSObject

obj = SFSObject()
obj.putInt('name', 12)
obj.putBool('name_2', False)

a: int = obj.getInt('name')
```

The example above creates an object obj of class SFSObject. The putInt and putBool methods are then used to add data to the object.

### 2. Working with arrays

```python
from pyfox2x.sfs_types.SFSArray import SFSArray

arr = SFSArray()
arr.addInt(12)
arr.addLong(324233242)

b: int = arr.getLong(1)
```

This example creates an array arr of the SFSArray class. Using the addInt and addLong methods, data is added to the array.

### 3. Connect to the server

```python
from pyfox2x.sfs_types.SFSObject import SFSObject
from pyfox2x.sfs_types.SFSArray import SFSArray
from pyfox2x.sfs_client import SFSClient

auth_params = SFSObject()
auth_params.putUtfString('user_id', 'Kmdinndsdfcn')
auth_params.putUtfString('client_os', '12')
auth_params.putUtfString('client_platform', 'android')
auth_params.putUtfString('client_version', '3.1.1')

client = SFSClient()
client.connect(host='127.0.0.1', port=9933)
client.send_login_request('ZoneName', 'username', 'password', auth_params)
```

This example creates an SFSObject auth_params object that adds various authorization parameters. Then a client object of the SFSClient class is created and a connection to the server is established, specifying the host and port. Finally, an authorization request is sent, specifying the zone name, username, password and authorization parameters.

### 4.  Communication with the server

In the pyFox2X library you can send requests to the server and expect responses from it. The following methods are available for this:

#### Sending a request and waiting for a response

```python
player_object = client.request('get_player_data', SFSObject().putLong("last_updated", 0)).get("player_object")
```

The example above sends a request with the name 'get_player_data' and the parameter 'last_updated'. A response is expected from the server, and the value of the "player_object" field from the response is written to the 'player_object' variable.

#### Sending a request without waiting for a response

```python
client.send_extension_request('collect_coins_from_monster', SFSObject().putLong("user_monster_id", 199))
```

This example sends a request with the name 'collect_coins_from_monster' and the parameter 'user_monster_id'. The request is sent to the server, but no response is expected.

#### Waiting for a response without sending a request

```python
response = client.wait_extension_response('giveaway')
```

This example expects a response from a server named 'giveaway'. The method blocks until the response is received, and the value of the response is written to the 'response' variable.

#### Waiting for one of several packages

```python
cmd, response = client.wait_requests(['login_success', 'login_failed', 'player_banned'])
```

This example expects one of the packets named 'login_success', 'login_failed', or 'player_banned' to arrive. The method blocks until one of the packets arrives, and then returns the name of the packet and its contents in the 'cmd' and 'response' variables, respectively.

These are just some examples of using methods to send requests and receive responses from the server. You can adapt them to suit your needs in your project.
