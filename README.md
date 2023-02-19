# SAFE Paket Weirdness

This repo demonstarates some weird behaviour that I've noticed when using Paket (v7.0.2) with SAFE Stack (v4.2.0).

It seems that, when the Shared project doesn't have a paket.references file, the Server project (which has a project reference to the Shared project) is able to use dependencies that are declared in the paket.dependencies file (), but not referred to in the Server project's paket.references file. For example, in ./src/Server/Server.fs, there is code relying on the Fake.Core package, and the project compiles. I was expecting that the paket.references would limit the packages available to the corresponding project from all of those available in the dependencies file to only those present in the references file, hence the project wouldn't build.

When the Shared project has its own paket.references file, the Server project no longer compiles after a `dotnet paket install`, complaining that "The namespace or module 'Fake' is not defined, which ties in with my expectation. You can see this on the fix branch.

I guess that what's happening here is that, because there is no references file, the Shared project refers to all packages declared in the dependencies file. As a result, the Server project is able to access them as transitive dependencies. However, when the Shared project has its own references file, the packages it refers to is limited, hence the Server project can no longer transitively access packages like Fake.Core.
