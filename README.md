# tracecmd-mkdot
A simple python script to generate dot files for vizualization of call graph from function call traces with `trace-cmd` command.

## Usage

~~~
usage: tracecmd-mkdot [-h] [-d OUTPUTDIR]
                      [-c FUNCTION | -f FUNCTION | -t FUNCTION]
                      FILE

positional arguments:
  FILE                  input file

optional arguments:
  -h, --help            show this help message and exit
  -d OUTPUTDIR, --dir OUTPUTDIR
                        output directory
  -c FUNCTION, --contain FUNCTION
                        select call paths conain the function
  -f FUNCTION, --from FUNCTION
                        select call paths from the function
  -t FUNCTION, --to FUNCTION
                        select call paths to the function
~~~

## Example

1. Record function call traces with `trace-cmd` command.

        
        # trace-cmd reset
        # trace-cmd record -p function dd if=/dev/zero of=testfile bs=30M count=1
        # trace-cmd report -O ftrace:parent=1 > trace-cmd_report.out
        

2. Run `tracecmd-mkdot` command with `-c scsi_request_fn` option. It select call paths contains `scsi_request_fn` function, and generate `.gv` files.

        
        # mkdir results
        # tracecmd-mkdot -d ./results -c scsi_request_fn ./trace-cmd_report.out
	

3. Run `dot` command for each `.gv` file to generate image files draw the selected call paths.

        
        # for FILE in $(ls *callpaths_*.gv); do echo "Processing $FILE"; dot -Tsvg -O $FILE; done
        
4. View the saved `.svg` files.
