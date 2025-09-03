desc 'Make a new post'
task :post, [:name] do |t, args|
  if args.name then
    template(args.name)
  else
    puts "Name required"
  end
end

def template(name)
  t = Time.now
  contents = "" # otherwise using it below will be badly scoped
  File.open("_posts/yyyy-mm-dd-template.markdown", "rb") do |f|
    contents = f.read
  end
  contents = contents.sub("%date%", t.strftime("%Y-%m-%d %H:%M:%S %z")).sub("%title%", name)
  filename = "_posts/" + t.strftime("%Y-%m-%d-") + name.downcase.gsub( /[^a-zA-Z0-9_\.]/, '-') + '.markdown'
  if File.exist? filename then
    puts "Post already exists: #{filename}"
    return
  end
  File.open(filename, "wb") do |f|
    f.write contents
  end
  puts "created #{filename}"
end

