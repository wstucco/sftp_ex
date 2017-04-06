### This is a fork of SftpEx module that fix a [bug in the upload](https://github.com/mikejdorm/sftp_ex/pull/4)
### I do not maintain it 
### Please use the original module and push for the approval of the [PR](https://github.com/mikejdorm/sftp_ex/pull/4) containing the fix

# SftpEx

An Elixir wrapper around the Erlang SFTP application. This allows for the use of Elixir Streams to 
transfer files via SFTP. 
 
## Creating a Connection

The following is an example of creating a connection with a username and password. 

    {:ok, conn} = SftpEx.connect([host: 'somehost', user: 'someuser', password: 'somepassword'])

Other connection arguments can be found in the [Erlang documentation]("http://erlang.org/doc/man/ssh.html#connect-3") 


## Streaming Files

An example of writing a file to a server is the following.
    
    stream = File.stream!("filename.txt")
        |> Stream.into(SftpEx.stream!(connection,"/home/path/filename.txt"))
        |> Stream.run
   
A file can be downloaded as follows - in this example a remote file "test2.csv" is downloaded to 
the local file "filename.txt" 

    SftpEx.stream!(connection,"test2.csv") |> Stream.into(File.stream!("filename.txt")) |> Stream.run

or using Enum.into

    SftpEx.stream!(connection, "test2.csv") |> Enum.into(File.stream!("filename.txt"))
    
This follows the same pattern as Elixir IO streams so a file can be transferred
from one server to another via SFTP as follows.

    stream = SftpEx.stream!(connection,"/home/path/filename.txt")
    |> Stream.into(SftpEx.stream!(connection2,"/home/path/filename.txt"))
    |> Stream.run

## Installation

If [available in Hex](https://hex.pm/docs/publish), the package can be installed as:

  1. Add `sftp_ex` to your list of dependencies in `mix.exs`:

    ```elixir
    def deps do
      [{:sftp_ex, "~> 0.2.1"}]
    end
    ```

  2. Ensure `sftp_ex` is started before your application:

    ```elixir
    def application do
      [applications: [:sftp_ex]]
    end
    ```

