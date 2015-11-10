# SandCastle and VS2015 Update 1

This repo contains reproductions for issues with
[SandCastle](https://github.com/EWSoftware/SHFB) running on Visual Studio 2015
Update 1.

First, in order to repro you need to restore the sandcastle nuget packages.
Make sure you have nuget.exe in your path (this can be downloaded from
http://dist.nuget.org/win-x86-commandline/latest/nuget.exe) and run
"restore.cmd".

Run the following commands from a "MSBuild Command Prompt for VS2015" window.

## Issue 1: BHT0001

It seems that whenever SandCastle runs on VS2015 Update 1 it generates a BHT0001
warning.  To see this, run:

    msbuild sc.shfbproj

After executing, this will display:

    "C:\prj\sc\sc.shfbproj" (default target) (1) ->
    (CoreBuildHelp target) ->
      SHFB : warning BHT0001: Unable to get executing project:  Unable to obtain matching project from the global collectio
    n.  The specified project will be loaded but command line property overrides will be ignored. [C:\prj\sc\sc.shfbproj]
    
        1 Warning(s)
        0 Error(s)


## Issue 2: Wildcards in <Image> item groups no longer work

The wildcards.shfbproj uses this technique to include all svg files from a directory:

    <WildcardImages Include="media\*.svg"/>
    <Image Include="@(WildcardImages)">
      <ImageId>%(Filename)</ImageId>
      <CopyToMedia>True</CopyToMedia>
    </Image>

This works fine on VS2015, but on VS2015 Update 1 the following happens:

    msbuild wildcards.shfbproj

      CopyConceptualContent
    SHFB : error BE0065: BUILD FAILED: Could not find file 'C:\prj\sc\@(WildcardImages)'. [C:\prj\sc\wildcards.shfbproj]
      Failed
      Build details can be found in C:\prj\sc\Output-wildcards\LastBuild.log
    Done Building Project "C:\prj\sc\wildcards.shfbproj" (default targets) -- FAILED.
