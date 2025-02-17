## Problem 

One or more processes have an optional input file. 

## Solution 

Use a special file name to mark the absence of the file parameter. 

## Code 

```groovy
params.inputs = "$baseDir/data/prots/*{1,2,3}.fa"
params.filter = 'NO_FILE'

process foo {
  debug true   
  input:
  path seq
  path opt

  script:
  def filter = opt.name != 'NO_FILE' ? "--filter $opt" : ''
  """
  echo your_commad --input $seq $filter
  """
}

workflow {
  prots_ch = Channel.fromPath(params.inputs, checkIfExists:true)
  opt_file = file(params.filter)

  foo(prots_ch, opt_file)
}
```

## Run it 

Run the script with the following command: 

```bash
nextflow run nextflow-io/patterns/optional-input.nf 
```

Run the same script providing an optional file input:

```bash
nextflow run nextflow-io/patterns/optional-input.nf --filter foo.txt
```
