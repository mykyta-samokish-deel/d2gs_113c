How to setup D2 game to support Unicode Char Name?

1. ClientSide Setup.
   a. You need install the full locale font support for you language. And add "-direct -txt" switch to your d2loader.exe cmd.
   b. You need the latest d2hackmap (20.04 and later).
      If you have no this d2hackmap, please copy the d2hack.script file to your D2 directory.
      This d2hack.script file will enable your enter unicode when create new char, and allow you display unicode char name in game.
      If you have the latest d2hackmap, the d2hack.script is no needed.

2. ServerSide Setup.
   a. Modify the D2CS program. You need modify the D2CS d2charfile.c, rewrite the d2char_check_charname function, allow the unicode char name can pass the check.
      Also, you can use the compiled version in this directory.
   b. Use the build 35(or greater) d2gs, and set EnableUnicodeCharName=1 in your d2server.ini(in [NewFeatures] section).



D2CS
d2charfile.c

int InvalidCharName(char thechar)
{
	// \/:*?"<>|
	if (thechar == '\\' || thechar == '/' || thechar == ':' || thechar == '*' || thechar == '?' || thechar == '"' || thechar == '<' || thechar == '>' || thechar == '|')
		return 1;
	return 0;
}

extern int d2char_check_charname(char const * name)
{
	unsigned int	i;
	unsigned char	ch;
	if (!name) return -1;
	if (InvalidCharName((int)name[0])) return -1;
	if (name[0] == '.') return -1;
	for (i=1; i<=MAX_CHARNAME_LEN; i++) {
		ch=name[i];
		if (ch=='\0') break;
		if (!InvalidCharName(ch)) continue;
		return -1;
	}
	if (i >= MIN_NAME_LEN || i<= MAX_CHARNAME_LEN) return 0;
	return -1;
}