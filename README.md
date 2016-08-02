# PlugLoggerJson

A comprehenisve JSON logger Plug.

## Dependencies
  * Plug
  * Poison

## Installation

If [available in Hex](https://hex.pm/docs/publish), the package can be installed as:

  1. Add plug_logger_json to your list of dependencies in `mix.exs`:

        def deps do
          [{:plug_logger_json, "~> 0.0.2"}]
        end

  2. Ensure plug_logger_json is started before your application:

        def application do
          [applications: [:plug_logger_json]]
        end
  3. Replace `Plug.Logger` with `Plug.LoggerJSON` in your plug pipeline (endpoint.ex for phoenix apps)

### Recommended Setup
  * Configure this application
    * Add to your `config/config.exs` or `config/env_name.exs`:

            config :plug_logger_json,
              filtered_keys: ["password", "authorization"],

  * Configure the logger (console)
    * Add to your `config/config.exs` or `config/env_name.exs`:

            config :logger, :console,
              format: "$message\n",
              level: :info,
              metadata: [:request_id]

  * Configure the logger (file)
    * Add `{:logger_file_backend, "~> 0.0.7"}` to your mix.exs
    * Run `mix deps.get`
    * Add to your `config/config.exs` or `config/env_name.exs`:

            config :logger, format: "$message\n", backends: [{LoggerFileBackend, :log_file}, :console]

            config :logger, :log_file,
              format: "$message\n",
              level: :info,
              metadata: [:request_id],
              path: "log/my_pipeline.log"

  * Ensure you are using Plug.Parsers (Phoenix adds this to endpoint.ex by default) to parse params & req bodies

            plug Plug.Parsers,
              parsers: [:urlencoded, :multipart, :json],
              pass: ["*/*"],
              json_decoder: Poison

## Contributing

Pull requests are welcomed. Before submitting your pull request, please run:
* `mix credo --strict`
* `mix coveralls`
* `mix dialyzer`

Please squash your pull request's commits into a single commit with a message and
description explaining the commit.
