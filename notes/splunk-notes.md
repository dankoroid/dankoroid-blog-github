---
If you rely on your support engineers to get some valuable data from Splunk logs - you need to ensure that Splunk will not truncate message.

It can be possible that after some length (idk, 4000 or smth) message will just be truncated - and there will be no way to get full message.

So, either review if there is any Splunk configuration for that - or split valuable message onto some parts

---
Write down about `eval`, `table`, `regex` - and they could be combined

---
Splunk - you can't search for wildcard as a literal - because it is a reserved symbol
There are some proposed workarounds, but not ready to investigate them yet

[Splunk docs](https://docs.splunk.com/Documentation/SCS/current/Search/Wildcards#:~:text=You%20can't%20search%20for,function%20to%20filter%20the%20results.)

---
