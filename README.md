# ffuflags (formerly parse_ffuf)

A way to generate filter flags for `ffuf` from a previous fuzz + more to come!

## Motivation

Typically, an `ffuf` user will use the `-ac` autocalibration flag in order to automatically apply filters based off of initial tests ffuf will do before beginning a full fuzz.

However, in my experience, I have found that the initial autocalibration ends up missing a lot of values that could be filtered-out. 

Manually-writing filter flags has been time-consuming, but I realized that I could automate the process with my current (and possibly overall) favorite programming language: Python!

What this does is generate a string representing flags that are to be passed to `ffuf`. 

I have a fuzzing script to help manage configuration options that I regularly pass into `ffuf`, so as an example in a script, you could do something like this:

```
ffuf -X GET -w $WORDLIST0 $FLAGS -u "$1/FUZZ" $HEADERS -o $OUTFILE_SMALL;
FILTER=$(python3 -m ffuflags -i "$OUTFILE_SMALL");
ffuf -X GET -w $WORDLIST1 $FLAGS $FILTER -u "$1/FUZZ" $HEADERS -o $OUTFILE_BIG;
```


## Usage

```
python3 -m ffuflags -v -i <input_file> [-s | -p | -c <status_code>]
python3 -m ffuflags --verbose --input <input_file> [--sort-by-value | --sort-by-param | --code <status_code>]
```

## Example

### Simple Usage

```
python3 -m ffuflags -i myfuzz.json 
```

### Filtering status codes from which flags you generate

```
python3 -m ffuflags -i myfuzz.json -c 200
python3 -m ffuflags -i myfuzz.json --code 200
python3 -m ffuflags -i myfuzz.json -c 200,404
python3 -m ffuflags -i myfuzz.json --code 200,404
```

### Print Extra Table of Results by Category (status code, length, duration, words, lines)

```
python3 -m ffuflags -v -i myfuzz.json 
python3 -m ffuflags --verbose -i myfuzz.json 
```

### Sorting the Extra Table of Results 

```
python3 -m ffuflags -v -s -i myfuzz.json 
python3 -m ffuflags -v --sort-by-value -i myfuzz.json 
python3 -m ffuflags -v --sort-by-param -i myfuzz.json 
```

-----

Did you find this tool helpful? 

I stream regularly on Twitch as well at: [https://www.twitch.tv/darkmage666](https://www.twitch.tv/evildojo666)

