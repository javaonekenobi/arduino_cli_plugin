package org.irio.arduinocli;

import org.apache.maven.plugin.AbstractMojo;
import org.apache.maven.plugin.MojoExecutionException;
import org.apache.maven.plugins.annotations.Mojo;
import org.apache.maven.plugins.annotations.Parameter;
import org.apache.maven.plugins.annotations.LifecyclePhase;

import java.io.File;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.LineNumberReader;


@Mojo(name="arduinocli", defaultPhase = LifecyclePhase.COMPILE, requiresProject=true)
public class	ArduinoCli	extends AbstractMojo
{
  @Parameter(property="arduinocli.cliexe", defaultValue="/usr/local/bin/arduino-cli-linux64")
  private String	cliexe;
  @Parameter(property="arduinocli.board", defaultValue="arduino:avr:micro")
  private String	board;
  @Parameter(property="arduinocli.sketch", defaultValue="MySketch")
  private String	sketch;
  @Parameter(property="arduinocli.directory", defaultValue="src/arduino")
  private String	directory;
  @Parameter(property="arduinocli.buildpath", defaultValue="target/arduino")
  private String	buildpath;

  private String[]	extensions	= { "elf", "hex" };

  private String	fileBaseFromSketch()
  {
    return sketch+"."+board.replaceAll(":",".");
  }

  private int	moveFile(String fn)
  {
    int	ret	= -1;

    try
    {
      File	f1;
      File	f2;

      f1 = new File(directory+"/"+sketch+"/"+fn);

      if (f1.exists())
      {
	f2 = new File(buildpath+"/"+sketch+"/"+fn);

	getLog().error("moving file "+f1+" to "+f2);

	new File(buildpath+"/"+sketch).mkdirs();

	f1.renameTo(f2);

	ret = 0;
      }
      else
      {
	getLog().error("file "+f1+" not found");
      }
    }
    catch (Throwable e)
    {
      getLog().error("exception "+e+" moving file "+fn);
    }

    return ret;
  }

  private int	moveFiles(String sketch)
  {
    String	fn	= fileBaseFromSketch();
    int		ret	= -1;

    for (int i=0; i<extensions.length; i++)
    {
      ret = moveFile(fn+"."+extensions[i]);
      if (ret != 0)
      {
	break;
      }
    }

    return ret;
  }

  public void	execute()
    		throws MojoExecutionException
  {
    getLog().info("Hello, I'm the empty arduinocli plugin, board is "+board);
    getLog().info("cliexe is "+cliexe);
    getLog().info("directory is "+directory);
    getLog().info("sketch is "+sketch);
    getLog().info("buildpath is "+buildpath);

    try
    {
      File	f1;
      File	f2;
      Process	proc;
      int	ret;

      proc = Runtime.getRuntime().exec(cliexe+" compile --format json --fqbn "+board+" "+directory+"/"+sketch);

      ret = proc.waitFor();

      if (ret != 0)
      {
	InputStream		is	= proc.getInputStream();
	InputStreamReader	isr	= new InputStreamReader(is);
	LineNumberReader	lnr	= new LineNumberReader(isr);
	String			line;

	getLog().error("command failed: "+ret);

	while ((line = lnr.readLine()) != null)
	{
	  getLog().error(line);
	}
      }
      else
      {
	getLog().info("moving files to "+buildpath);

	ret = moveFiles(sketch);

	if (ret != 0)
	{
	  getLog().error("failed to move files to "+buildpath);
	}
      }
    }
    catch (Throwable e)
    {
      getLog().error("exception "+e);
      throw new MojoExecutionException("failed to execute "+e);
    }
  }
}
