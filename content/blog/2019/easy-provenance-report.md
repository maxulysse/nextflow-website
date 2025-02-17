title=Easy provenance reporting
date=2019-08-29
type=post
tags=nextflow,resume
status=published
author=Evan Floden
icon=evan.jpg
~~~~~~

*Continuing our [series on understanding Nextflow resume](blog/2019/demystifying-nextflow-resume.html), we wanted to delve deeper to show how you can report which tasks contribute to a given workflow output.*

### Easy provenance reports


When provided with a run name or session ID, the log command can return useful information about a pipeline execution. This can be composed to track the provenance of a workflow result.

When supplying a run name or session ID, the log command lists all the work directories used to compute the final result. For example:

```
$ nextflow log tiny_fermat

/data/.../work/7b/3753ff13b1fa5348d2d9b6f512153a
/data/.../work/c1/56a36d8f498c99ac6cba31e85b3e0c
/data/.../work/f7/659c65ef60582d9713252bcfbcc310
/data/.../work/82/ba67e3175bd9e6479d4310e5a92f99
/data/.../work/e5/2816b9d4e7b402bfdd6597c2c2403d
/data/.../work/3b/3485d00b0115f89e4c202eacf82eba
```

Using the option `-f` (fields) it’s possible to specify which metadata should be printed by the log command. For example:

```
$ nextflow log tiny_fermat -f 'process,exit,hash,duration'

index	0	7b/3753ff	2s
fastqc	0	c1/56a36d	9.3s
fastqc	0	f7/659c65	9.1s
quant	0	82/ba67e3	2.7s
quant	0	e5/2816b9	3.2s
multiqc	0	3b/3485d0	6.3s
```

The complete list of available fields can be retrieved with the command:

```
$ nextflow log -l
```

The option `-F` allows the specification of filtering criteria to print only a subset of tasks. For example:

```
$ nextflow log tiny_fermat -F 'process =~ /fastqc/'

/data/.../work/c1/56a36d8f498c99ac6cba31e85b3e0c
/data/.../work/f7/659c65ef60582d9713252bcfbcc310
```

This can be useful to locate specific tasks work directories.

Finally, the `-t` option allows for the creation of a basic custom HTML provenance report that can be generated by providing a template file, in any format of your choice. For example:

```
<div>
<h2>${name}</h2>
<div>
Script:
<pre>${script}</pre>
</div>

<ul>
    <li>Exit: ${exit}</li>
    <li>Status: ${status}</li>
    <li>Work dir: ${workdir}</li>
    <li>Container: ${container}</li>
</ul>
</div>
```

By saving the above snippet in a file named template.html, you can run the following command:

```
$ nextflow log tiny_fermat -t template.html > provenance.html
```

Open it in your browser, et voilà! 

## Conclusion 

This post introduces a little know Nextflow feature and it's intended to show how it can be used  
to produce a custom execution report reporting some - basic - provenance information. 

In future releases we plan to support a more formal provenance specification and execution tracking features.



