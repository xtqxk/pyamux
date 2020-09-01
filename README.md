# pyamux
A python port of yamux 

Usage and example:

```python
# coding:utf-8
import asyncio
import sys
import sys
import socket
from pyamux import stream
from pyamux import Server
from pyamux import Client


async def handle_echo(reader, writer):
    print("start server handler")
    conn = stream.Conn(reader, writer)
    session = Server(conn, None)
    await asyncio.gather(session.init(), new_client(session))


async def new_client(session):
    _stream, err = await session.accept()
    if err is not None:
        raise err
    while True:
        ret = await _stream.read(4)


async def server():
    print("start server")
    srv = await asyncio.start_server(handle_echo, "127.0.0.1", 8089, loop=loop)


async def client_init(session):
    _stream, err = session.Open()
    if err is not None:
        raise err
    ret, err = await _stream.write(b"ping")
    print("start send,%s" % ret)
    ret, err = await _stream.write(b"pong")
    print("start send,%s" % ret)


async def client():
    print("start client")
    reader, writer = await asyncio.open_connection("127.0.0.1", 8089)
    conn = stream.Conn(reader, writer)
    session = Client(conn, None)
    await asyncio.gather(session.init(), client_init(session))


if __name__ == "__main__":
    loop = asyncio.get_event_loop()
    loop.run_until_complete(server())
    loop.run_until_complete(client())
    loop.run_forever()

```
