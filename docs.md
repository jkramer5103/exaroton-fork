# Exaroton Python Library Documentation

This documentation provides an overview of the `exaroton` Python library, which allows you to interact with the Exaroton API.

## Installation

(Assuming standard Python package installation)
```bash
pip install exaroton
```

## Usage

First, import the `Exaroton` class and initialize it with your API token. You can obtain your API token from your [Exaroton account page](https://exaroton.com/account/).

```python
from exaroton import Exaroton

# Replace 'YOUR_API_TOKEN' with your actual Exaroton API token
exaroton_api = Exaroton(token="YOUR_API_TOKEN")
```

## `Exaroton` Class

The `Exaroton` class provides methods to interact with various aspects of the Exaroton API.

### Initialization

`Exaroton(token: str, host: str = "https://api.exaroton.com/v1")`
- `token` (str): Your authentication token from the [user page](https://exaroton.com/account/).
- `host` (str, optional): The API host. Defaults to "https://api.exaroton.com/v1".

### Account Endpoints

#### `get_account()`
Retrieves information about the authenticated account.
- **Returns**: `types.Account` - An object containing your account details.

```python
account_info = exaroton_api.get_account()
print(f"Account Name: {account_info.name}")
print(f"Credits: {account_info.credits}")
```

### Server Endpoints

#### `get_servers()`
Retrieves a list of all servers associated with your account.
- **Returns**: `List[types.Server]` - A list of `Server` objects.

```python
servers = exaroton_api.get_servers()
for server in servers:
    print(f"Server Name: {server.name}, Status: {server.status}")
```

#### `get_server(id: str)`
Retrieves details for a specific server by its ID.
- `id` (str): The ID of the server.
- **Returns**: `types.Server` - The `Server` object.

```python
server_id = "YOUR_SERVER_ID"
server = exaroton_api.get_server(server_id)
print(f"Server {server.name} is {server.status}")
```

#### `get_server_logs(id: str)`
Retrieves the current log file content for a specified server.
- `id` (str): The ID of the server.
- **Returns**: `str` - The entire log file content as a string.

```python
logs = exaroton_api.get_server_logs(server_id)
print("Server Logs:\n", logs)
```

#### `upload_logs(id: str)`
Uploads the server logs to [mclo.gs](https://mclo.gs) and returns the identifier and URLs.
- `id` (str): The ID of the server.
- **Returns**: `types.Logs` - An object containing the log ID and URLs.

```python
uploaded_logs = exaroton_api.upload_logs(server_id)
print(f"Uploaded Logs URL: {uploaded_logs.url}")
```

#### `get_server_ram(id: str)`
Retrieves the currently set RAM for a specified server.
- `id` (str): The ID of the server.
- **Returns**: `int` - RAM in Gigabytes.

```python
ram = exaroton_api.get_server_ram(server_id)
print(f"Server RAM: {ram} GB")
```

#### `set_server_ram(id: str, ram: int)`
Sets a new amount of RAM to be used by the specified server.
- `id` (str): The ID of the server.
- `ram` (int): RAM in Gigabytes.
- **Returns**: `int` - The newly set RAM in Gigabytes.

```python
new_ram = exaroton_api.set_server_ram(server_id, 4) # Set to 4GB
print(f"New Server RAM: {new_ram} GB")
```

#### `start(id: str)`
Starts the specified server.
- `id` (str): The ID of the server.
- **Returns**: `str` - A confirmation message (e.g., "Hello, world!").

```python
response = exaroton_api.start(server_id)
print(response)
```

#### `stop(id: str)`
Stops the specified server.
- `id` (str): The ID of the server.
- **Returns**: `str` - A confirmation message (e.g., "Hello, world!").

```python
response = exaroton_api.stop(server_id)
print(response)
```

#### `restart(id: str)`
Restarts the specified server.
- `id` (str): The ID of the server.
- **Returns**: `str` - A confirmation message (e.g., "Hello, world!").

```python
response = exaroton_api.restart(server_id)
print(response)
```

#### `command(id: str, command: str)`
Sends a command to the specified server's console.
- `id` (str): The ID of the server.
- `command` (str): The command to send (e.g., "say Hello World").
- **Returns**: `str` - A confirmation message (e.g., "Hello, world!").

```python
response = exaroton_api.command(server_id, "say Hello from Exaroton API!")
print(response)
```

### Player List Endpoints

#### `get_player_lists(id: str)`
Retrieves a list of available player lists for a server (e.g., "whitelist", "ops").
- `id` (str): The ID of the server.
- **Returns**: `List[str]` - A list of available player list names.

```python
player_lists = exaroton_api.get_player_lists(server_id)
print(f"Available Player Lists: {player_lists}")
```

#### `get_player_list(id: str, player_list: str)`
Retrieves the players on a specific player list.
- `id` (str): The ID of the server.
- `player_list` (str): The name of the player list (e.g., "whitelist").
- **Returns**: `List[str]` - A list of player usernames on that list.

```python
whitelist_players = exaroton_api.get_player_list(server_id, "whitelist")
print(f"Whitelist Players: {whitelist_players}")
```

#### `add_player_to_list(id: str, player_list: str, usernames: list)`
Adds one or more players to a specified player list.
- `id` (str): The ID of the server.
- `player_list` (str): The name of the player list (e.g., "whitelist").
- `usernames` (list): A list of usernames to add.
- **Returns**: `List[str]` - The updated list of players on that list.

```python
updated_whitelist = exaroton_api.add_player_to_list(server_id, "whitelist", ["Player1", "Player2"])
print(f"Updated Whitelist: {updated_whitelist}")
```

#### `remove_player_from_list(id: str, player_list: str, usernames: List[str])`
Removes one or more players from a specified player list.
- `id` (str): The ID of the server.
- `player_list` (str): The name of the player list (e.g., "whitelist").
- `usernames` (list): A list of usernames to remove.
- **Returns**: `List[str]` - The updated list of players on that list.

```python
updated_whitelist = exaroton_api.remove_player_from_list(server_id, "whitelist", ["Player1"])
print(f"Updated Whitelist after removal: {updated_whitelist}")
```

### File Endpoints

#### `get_file_data(id: str, path: str)`
Retrieves the content of a file on the server.
- `id` (str): The ID of the server.
- `path` (str): The path to the file (e.g., "world/level.dat").
- **Returns**: The content of the file. The return type depends on the file's content type (e.g., `str` for text files, `bytes` for binary files).

```python
file_content = exaroton_api.get_file_data(server_id, "server.properties")
print("server.properties content:\n", file_content)
```

#### `write_file_data(id: str, path: str, data)`
**Note**: This method is currently not implemented in the library.
Attempts to write content to a file on the server. If the file does not exist, it will be created.
- `id` (str): The ID of the server.
- `path` (str): The path to the file.
- `data`: The content to write to the file.

#### `delete_file_data(id: str, path: str)`
Deletes a file on the server.
- `id` (str): The ID of the server.
- `path` (str): The path to the file.
- **Returns**: The API response data.

```python
# Be careful when deleting files!
# response = exaroton_api.delete_file_data(server_id, "path/to/your/file.txt")
# print(response)
```

### Credit Pool Endpoints

#### `get_credit_pools()`
Retrieves a list of credit pools the authenticated account is a member of.
- **Returns**: `List[types.CreditPool]` - A list of `CreditPool` objects.

```python
credit_pools = exaroton_api.get_credit_pools()
for pool in credit_pools:
    print(f"Credit Pool: {pool.name}, Credits: {pool.credits}")
```

#### `get_credit_pool(id: str)`
Retrieves details for a specific credit pool by its ID.
- `id` (str): The ID of the credit pool.
- **Returns**: `types.CreditPool` - The `CreditPool` object.

```python
pool_id = "YOUR_POOL_ID"
credit_pool = exaroton_api.get_credit_pool(pool_id)
print(f"Credit Pool {credit_pool.name} has {credit_pool.credits} credits.")
```

#### `get_credit_pool_members(id: str)`
Retrieves a list of members for a specified credit pool.
- `id` (str): The ID of the credit pool.
- **Returns**: `List[types.CreditPoolMember]` - A list of `CreditPoolMember` objects.

```python
pool_members = exaroton_api.get_credit_pool_members(pool_id)
for member in pool_members:
    print(f"Member: {member.name}, Share: {member.share}")
```

#### `get_credit_pool_servers(id: str)`
Retrieves a list of servers associated with a specified credit pool.
- `id` (str): The ID of the credit pool.
- **Returns**: `List[types.Server]` - A list of `Server` objects.

```python
pool_servers = exaroton_api.get_credit_pool_servers(pool_id)
for server in pool_servers:
    print(f"Server in Pool: {server.name}")
```

## `types` Module

The `types` module defines the data structures used by the `exaroton` library to represent API responses.

### `List`
A custom list class used for beautifying output.

### `ExarotonType`
Base class for all Exaroton data types, providing common serialization methods.

### `Account`
Represents an Exaroton account.
- `name` (str): Account name.
- `email` (str): Account email.
- `verified` (bool): Whether the account is verified.
- `credits` (int): Available credits.

### `Server`
Represents an Exaroton server.
- `id` (str): Server ID.
- `name` (str): Server name.
- `address` (str): Server address.
- `motd` (str): Message of the Day.
- `status` (str): Server status (e.g., "Online", "Offline", "Starting").
- `host` (str): Server host.
- `port` (int): Server port.
- `players` (`types.Players`): Player information.
- `software` (`types.Software`): Software information.
- `shared` (bool): Whether the server is shared.

### `Software`
Represents the software running on a server.
- `id` (str): Software ID.
- `name` (str): Software name (e.g., "Vanilla", "Spigot").
- `version` (str): Software version.

### `Players`
Represents player statistics for a server.
- `max` (int): Maximum player slots.
- `count` (int): Current player count.
- `list` (list): List of online player usernames.

### `Logs`
Represents information about uploaded logs.
- `id` (str): Log ID.
- `url` (str): URL to the uploaded log on mclo.gs.
- `raw` (str): Raw URL to the log.

### `CreditPool`
Represents a credit pool.
- `id` (str): Credit pool ID.
- `name` (str): Credit pool name.
- `credits` (float): Total credits in the pool.
- `servers` (int): Number of servers in the pool.
- `owner` (str): Owner's account name.
- `isOwner` (bool): True if the authenticated account is the owner.
- `members` (int): Number of members in the pool.
- `ownShare` (float): Your share of the pool.
- `ownCredits` (float): Your credits in the pool.

### `CreditPoolMember`
Represents a member of a credit pool.
- `account` (str): Member's account ID.
- `name` (str): Member's account name.
- `share` (float): Member's share of the pool.
- `credits` (int): Member's credits in the pool.
- `isOwner` (bool): True if this member is the owner.