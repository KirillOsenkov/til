# How to include commit URL in nuget package description

It's quite helpful to include a [direct link to the source code](https://www.nuget.org/packages/Microsoft.AspnetCore.Authorization) that produced a given package version, so your users can quickly see what's included \(i.e. they may be tracking a bug fix\). 

If you are using [Microsoft.SourceLink](https://www.nuget.org/packages?q=Microsoft.SourceLink), you already have all the information you need automatically populated for you. The following target will update the description just before the nuget is produced:

```markup
  <Target Name="UpdatePackageMetadata" 
          BeforeTargets="PrepareForBuild;GetAssemblyVersion;GenerateNuspec;Pack"
          Condition="'$(RepositoryUrl)' != '' And '$(SourceRevisionId)' != ''">
    <PropertyGroup>
      <Description>
        $(Description)

        Built from $(RepositoryUrl)/tree/$(SourceRevisionId.Substring(0, 9))
      </Description>
      <!-- Update nuspec properties too -->
      <PackageProjectUrl>$(RepositoryUrl)</PackageProjectUrl>
      <PackageDescription> $(Description)</PackageDescription>
    </PropertyGroup>
  </Target>
```

This will result in a short-ish link \(we trim it to 9 chars, which is the common short sha in Git\) to the repo, similar to many [ASP.NET Core](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) [packages](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Core/).

If you're using [GitInfo](https://www.nuget.org/packages/GitInfo/) instead, you'll have to populate the `RepositoryUrl` yourself, and replace `SourceRevisionId` with `GitSha`.

