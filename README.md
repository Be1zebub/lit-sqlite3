QOL fork of [lit-sqlite3](https://github.com/SinisterRectus/lit-sqlite3)
============================

adds a easy to use interface for prepared statements: 

## API

`conn:Query(query, args, opts)`  
query (string): is command for statement  
args (table): bind params  
> opts (table): options list  
  - all (boolean): fetch rows
  - row (boleean): fetch single row
  - value (boolean or key): fetch row, return value by key or next value
  - insert (boolean): fetch insert id 

## Usage

```lua
local sqlite = require("sqlite3")
local db = sqlite.open("users.db")

db:Query([[CREATE TABLE IF NOT EXISTS `users` (
    `id` INTEGER PRIMARY KEY,
    `steamid` TEXT NOT NULL,
    `name` TEXT NOT NULL,
    `registered` INTEGER NOT NULL
);]]) -- just commit, return nothing

local users = {}

function users:GetAll()
	return db:Query("SELECT * FROM `users`;", nil, {all = true}) -- return all rows
end

function users:Find(steamid)
	return db:Query("SELECT * FROM `users` WHERE `steamid` = ?;", {steamid}, {row = true}) -- return single row
end

function users:Register(steamid, name)
	return db:Query("INSERT INTO `users` (`steamid`, `name`, `registered`) VALUES (?, ?, ?);", {steamid, name, os.time()}, {insert = true}) -- return insert id
end

function users:ChangeName(steamid, name)
	db:Query("UPDATE `users` SET `name` = ? WHERE `steamid` = ?;", {name, steamid}) -- just commit, return nothing
end

function users:Wipe()
  db:Query("DELETE FROM `users`;") -- just commit, return nothing
end

p("Wipe", users:Wipe())
p("Register", users:Register("123", "me"))
p("GetAll", users:GetAll())
p("Find", users:Find("123"))
p("ChangeName", users:ChangeName("123", "me2"))
p("Find", users:Find("123"))

conn:close()
```

## Install

1. Install sqlite:
- Linux: `sudo apt-get install sqlite3 libsqlite3-dev` 
- Windows: https://www.sqlite.org/download.html
2. Put binaries to your project directory, or add it to a PATH
3. put lib in `deps` or `libs`

## Documentation

Need more docs? Read lit-sqlite3 readme.md & scilua ljsqlite3 docs
