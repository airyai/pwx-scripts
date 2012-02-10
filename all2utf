#!/usr/bin/env python
import chardet
import sys, os, fnmatch

print ('all2utf [pattern] [recursive]')
print ('')

VALV = 0.9

REC_TABLE = set(['on', 'true', 'yes', '-r', 'recursive'])
pattern = '*' if len(sys.argv) < 2 else sys.argv[1]
recursive = 'off' if len(sys.argv) < 3 else sys.argv[2]
if (pattern.lower() in REC_TABLE):
	recursive = pattern
	pattern = '*'
recursive = recursive.lower() in REC_TABLE

def f(path):
	for p in os.listdir(path):
		n = p
		p = os.path.join(path, p)
		if (os.path.isdir(p)):
			if (recursive):
				f(p)
		else:
			if (fnmatch.fnmatch(n, pattern)):
				g(p)

REP_ENCODING = {'gb2312': 'gb18030', 'gbk': 'gb18030'}
def g(path):
	try:
		cnt = None
		output = None
		with open(path, 'rb') as f:
			cnt = f.read()
			enc = chardet.detect(cnt[:1000])
			enc['encoding'] = REP_ENCODING.get(enc['encoding'].lower(), enc['encoding'])
			if (enc['confidence'] >= VALV):
				if (enc['encoding'] == 'utf-8'):
					print ('skip\tutf-8\t{1}\t{0}.'.format(path, enc['confidence']))
				else:
					output = unicode(cnt, enc['encoding']).encode('utf-8')
					print ('convert\t{1}\t{2}\t{0}.'.format(path, enc['encoding'], enc['confidence']))
			else:
				print ('FAIL!!\t{1}\t{2}\t{0}.'.format(path, enc['encoding'], enc['confidence']))
		if (output is not None):
			with open(path, 'wb') as f:
				f.write(output)
	except Exception as e:
		print ('{0}: error: {1}.'.format(path, e))

f('.')
