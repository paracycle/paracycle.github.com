require "cgi"
require "erb"
require "tmpdir"
require "yaml"

OG_TEMPLATE = "_og/template.html.erb"
OG_OUTPUT = "assets/images/og-image.png"
OG_SIZE = [1200, 630].freeze

desc "Regenerate #{OG_OUTPUT} from _config.yml"
task :og_image do
  require "ferrum"

  config = YAML.load_file("_config.yml")

  values = {
    host: config.fetch("domain") { config.fetch("url").sub(%r{\Ahttps?://}, "") },
    nav_links: config.fetch("navigation", [])
      .map { |item| item.fetch("title").downcase }
      .reject { |title| title == "home" },
    name_lines: config.fetch("title").split(" ", 2),
    tagline_parts: config.fetch("tagline", config.fetch("description")).split(" → "),
    footer: config.fetch("description").downcase.sub(" - ", " · ").sub(" at ", " @ "),
    avatar: "file://#{File.expand_path(config.fetch('avatar').delete_prefix('/'))}",
  }.transform_values do |value|
    case value
    when String then CGI.escapeHTML(value)
    when Array then value.map { |item| CGI.escapeHTML(item) }
    else value
    end
  end

  html = ERB.new(File.read(OG_TEMPLATE), trim_mode: "-").result_with_hash(values)

  Dir.mktmpdir do |dir|
    page = File.join(dir, "og-image.html")
    File.write(page, html)

    # `browser_path:` is only needed when Chrome isn't auto-detected;
    # set CHROME_BIN to override.
    browser = Ferrum::Browser.new(
      window_size: OG_SIZE,
      browser_path: ENV["CHROME_BIN"]
    )
    begin
      browser.go_to("file://#{page}")
      browser.network.wait_for_idle
      # full: captures the whole 1200x630 body regardless of how much
      # window chrome the headless viewport loses.
      browser.screenshot(path: OG_OUTPUT, full: true)
    ensure
      browser.quit
    end
  end

  puts "Wrote #{OG_OUTPUT}"
end
