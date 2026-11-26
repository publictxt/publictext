
- [Open Telecom Platform](Open%20Telecom%20Platform)
- [Erlang backpressure](Erlang%20backpressure.md)
- [Erlang gen_server](Erlang%20gen_server)


## setup on Fedora
1. **Install** with `sudo dnf install erlang rebar3`. Fedora's packages are fairly current. If you want a specific newer version later, use **mise** or **asdf** instead.
2. **Check** that it works: run `erl` to open the Erlang shell, and quit with `q().`
3. **Create a project** with `rebar3 new app follower`. That gives you an OTP application skeleton with a supervisor already in place.
4. **Run it** with `rebar3 shell` inside the project folder. It compiles everything and drops you into a shell with your app loaded.
5. **Editor:** in VS Code, install the **Erlang LS** extension for completion, go-to-definition and inline errors.