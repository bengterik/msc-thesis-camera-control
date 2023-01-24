FROM ubuntu:18.04

# Dockerfile for changing the LU-CS-EX numbers in MSc reports in PDF format
# Start by doing this once in the command line: docker build . -t lucs
# Then to run: docker run --rm -it -v /Your/full/path/toPDF/directory:/tmp lucs
# Note that the YOUR..full..path.. directory will also contain the outputs

RUN apt-get update && apt-get -y install \
  bash \
  make \
  perl

# required for working with the perl pdf utilities
RUN cpan install CAM::PDF

RUN mkdir /app
WORKDIR /app

RUN echo "#!/bin/bash\n" \
         "echo Report number update utility. Needs a PDF file and changes all occurrences of a string with another.\n" \
         "cont=y\n" \
         "while [ \$cont != 'n' ]\n" \
         "do\n" \
         "  read -p 'Report file:' pdffile\n" \
         "  read -p 'Look for [default is 2023-79, Enter to keep]:' rep \n" \
         "  rep=\${rep:-'2023-79'}\n" \
         "  read -p 'Wanted number:' numb\n" \
         "  echo Next is an indicator of whether the report number update will work.\n" \
         "  echo The text on the first page of the report should follow here:\n" \
         "  echo ----------------------------------------------------------------\n" \
         "  getpdftext.pl \$pdffile 1\n" \
         "  echo ----------------------------------------------------------------\n" \
         "  echo If that was gibberish, the next step will not work. Ask the students to change the number themselves.\n" \
         "  changepagestring.pl -v \$pdffile \$rep \"\$numb\" /tmp/LU-CS-EX-\$numb.pdf\n" \
         "  read -p 'Another? [y]/n ' cont\n" \
         "  cont=\${cont:-y}\n" \
         "done\n" > script.sh

RUN chmod +x script.sh

WORKDIR /tmp
CMD ["/app/script.sh"]
