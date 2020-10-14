# Blackout Documentation

## Improving The Documentation
Steps to get set up are:
- Download [docfx](https://github.com/dotnet/docfx/releases)
- Extra folder to somewhere. I just put the folder in `C:\docfx`
- Set up as PATH variable. _Please ask if you don't know how to do this._ You may need to reboot Windows at this point.
- [Clone repo](https://gitlab.com/blackout-sports/blackout-docs) to the same directory as `core-unity` and all other blackout projects. This is because the build paths rely on them being in the same directory.
- You can now use `docfx` to build and `docfx --serve` to run the docs on a server `localhost:8080` usually.
- Any extra information see [official website](https://dotnet.github.io/docfx/)
