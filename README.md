# arduino_cli_plugin
A Maven plugin for the Arduino CLI

this is a very rough attempt to create a maven plugin for the newly released arduino CLI.
At the moment it just handles the sketch compilation.
I only tested it on linux, not sure it will work on other platforms.

It's work in progress, but I hope it's a decent base if anybody feels like extending it or making something better.


to use it with maven, install it then use it in your pom.xml

for example

<plugins>
  <plugin>
    <groupId>org.irio</groupId>
    <artifactId>arduinocli</artifactId>
    <version>1.0</version>
    <executions>
      <execution>
        <id>execution1</id>
        <phase>compile</phase>
        <goals>
          <goal>arduinocli</goal>
        </goals>
      </execution>
    </executions>
    <configuration>
      <board>arduino:avr:micro</board>
      <sketch>MySketch</sketch>
      <directory>src/main/arduino</directory>
    </configuration>
  </plugin>
....
          
