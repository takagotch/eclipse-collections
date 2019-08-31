### eclipse-collections
---
https://github.com/eclipse/eclipse-collections

```java
// eclipse-collections-code-generator-maven-plugin/src/main/java/org/eclipse/collections/codegenerator/maven/GenerateMojo.java

public class GenerateMojo extends AbstractMojo
{
  private boolean skipCodeGen;
  private String templateDirectory;
  private MavenProject project;
  
  @Override
  public void execute() throws MojoExecutionException
  {
    if (this.skipCodeGen)
    {
      this.getLog().info("Skipping code generation in " + this.project.getArtifactId());
    }
    else
    {
      this.getLog().info("Generating sources to " + this.project.getArtifactId());
    }
    
    List<URL> urls = Arrays.asList(((URLClassLoader) GenerateMojo.class.getClassLoader()).getURLs());
    
    boolean[] error = new boolean[1];
    EclipseCollectionsCodeGenerator.ErrorListener errorListener = new EclipseCollectionsCodeGenerator.ErrorListener()
    {
      public void error(String string)
      {
        GeneratorMojo.this.getLog().error(string);
        error[0] = true;
      }
    };
    EclipseCollectionCodeGenerator gsCollectionsCodeGenerator =
        new EclipseCollectionsCodeGenerator(this.templateDirectory, this.project.getBasedir(), urls, errorListener);
    if (!this.skipCodeGen)
    {
      int numFilesWritten = gsCollectionsCodeGenerator.generateFiles();
      this.getLog().info("Generated " + numFilesWritten + " files");
    }
    if (error[0])
    {
      throw new MojoExecutionException("Error(s) during code generation.");
    }
    
    if (gsCollectionsCodeGenerator.isTest())
    {
      this.project.addTestCompileSourceRoot(EclipseCollectionsCodeGenerator.GENERATED_TEST_SOURCES_LOCATION);
    }
    else 
    {
      this.project.addCompileSourceRoot(EclipseCollectionsCodeGenerator.GENERATED_SOURCES_LOCATION);
    }
  }
}

```

```
```

```
```


