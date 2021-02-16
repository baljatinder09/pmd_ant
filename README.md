## PMD
## Description
Runs a set of static code analysis rules on some source code files and generates a list of problems found

## Installation
First you need to download PMD’s binary distribution zip file. Then you can either copy all “*.jar” files from PMD’s lib folder into one of ANT’s library folders (ANT_HOME/lib, ${user.home}/.ant/lib) or using the -lib command line parameter.
However, the preferred way is to define a <classpath> for pmd itself and use this classpath when adding the PMD Task. Assuming, you have extracted the PMD zip file to /home/joe/pmd-bin-6.31.0, then you can make use of the PMD Task like this:
```
<taskdef name="pmd" classname="net.sourceforge.pmd.ant.PMDTask">
    <classpath>
        <fileset dir="/home/joe/pmd-bin-6.31.0/lib">
            <include name="*.jar"/>
        </fileset>
    </classpath>
</taskdef>
```
Alternatively, a path can be defined and used via ``` classpathref ```
``` 
<path id="pmd.classpath">
    <fileset dir="/home/joe/pmd-bin-6.31.0/lib">
        <include name="*.jar"/>
    </fileset>
</path>
<taskdef name="pmd" classname="net.sourceforge.pmd.ant.PMDTask" classpathref="pmd.classpath" />
``` 
The examples below won’t repeat this taskdef element, as this is always required.

```
<target name="pmd">
    <taskdef name="pmd" classname="net.sourceforge.pmd.ant.PMDTask"/>
    <pmd shortFilenames="true">
        <ruleset>rulesets/java/quickstart.xml</ruleset>
        <ruleset>config/my-ruleset.xml</ruleset>
        <fileset dir="/usr/local/j2sdk1.4.1_01/src/">
            <include name="java/lang/*.java"/>
        </fileset>
    </pmd>
</target>
```
## Full Working example 
```
<project name="demo" default="pmd" basedir=".">
	<property name="build.dir" value="build"/>
	<property name="src.dir" value="src"/>
	<!-- Lib for PMD -->
	<path id="pmd.classpath">
		<fileset dir="C:\software\pmd-bin-6.31.0\lib">
			<include name="*.jar"/>
		</fileset>
	</path>
	<!-- Task for PMD-->
	<taskdef name="pmd" classname="net.sourceforge.pmd.ant.PMDTask" classpathref="pmd.classpath">
	</taskdef>
	<!-- PMD Target -->
	<target name="pmd">
		<pmd shortFilenames="true" failOnRuleViolation="true">
			<ruleset>${src.dir}/fis_rules.xml</ruleset>
			<formatter type="summaryhtml" toFile="${build.dir}/pmd_report.html"/>
			<sourceLanguage name="java" version="1.8"/>
			<fileset dir="..\VCPP_Services\${src.dir}">
				<include name="**/*.java"/>
			</fileset>
			<fileset dir="${src.dir}">
				<include name="**/*.java"/>
			</fileset>
		</pmd>
	</target>
</project>
```
## XML Rule Set
```
<?xml version="1.0"?>
<ruleset name="quickstart" xmlns="http://pmd.sourceforge.net/ruleset/2.0.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://pmd.sourceforge.net/ruleset/2.0.0 https://pmd.sourceforge.io/ruleset_2_0_0.xsd">
	<description>Quickstart configuration of PMD. Includes the rules that are most likely to apply everywhere.</description>
	<!-- <rule ref="category/java/codestyle.xml/ControlStatementBraces"/> <rule 
		ref="category/java/codestyle.xml/DuplicateImports"/> <rule ref="category/java/bestpractices.xml/UnusedImports"/> -->
	<rule ref="category/java/bestpractices.xml/UnusedLocalVariable" />
	<rule ref="category/java/bestpractices.xml/UnusedPrivateField" />
	<rule ref="category/java/bestpractices.xml/UnusedPrivateMethod" />
</ruleset>
```
