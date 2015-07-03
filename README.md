# pryman
Testing out a way to snapshot pry sessions

# How it works

Paste this into your ".pryrc" file via vim:

```ruby
Pry::Commands.block_command "batman", "Save the session" do |filename|
  filename ||= "batman.rb"
  filename = Pathname.new filename

  while File.exist? filename
    no_ext      = filename.basename.sub_ext('').to_s
    counter     = no_ext[/\d+$/].to_i.next
    incremented = Pathname.new(no_ext.sub /\d*$/, counter.to_s)
    filename    = filename.dirname + incremented.sub_ext(filename.extname)
  end

  inout = _pry_.input_array.to_a.zip(_pry_.output_array.to_a)
  body  = inout.map { |input, output|
    inspected_output = output.inspect.gsub(/^/, '  # ')
    "#{input}#{inspected_output}"
  }.join("\n")
  File.write(filename, body)
end
```

Say you are learning a new concept and testing it out in pry, but you want to save it to a ".rb" file.

Now you just type in

    $ batman

It will write/ovewrite a file called batman.rb

If you leave a text editor open, everytime you run "batman" you will see it overwrite the batman.rb file.

    Say you are using atom
    Cmd + Shift + S will now save a new file so that you can overwrite batman.rb again
    Then in terminal you launch batman.rb via atom
    atom batman.rb
    And you can keep logs of your progress
    Must exit pry sessions for "new" batman.rb file to be created

I just "Save As" into this repo to keep track

Excited to Start Turing on MONDAY!

# Many Thanks to Josh Cheek from Turing for helping me out.

