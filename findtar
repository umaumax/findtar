#!/usr/bin/env bash

BLACK="\033[0;30m" RED="\033[0;31m" GREEN="\033[0;32m" YELLOW="\033[0;33m" BLUE="\033[0;34m" PURPLE="\033[0;35m" LIGHT_BLUE="\033[0;36m" WHITE="\033[0;37m" GRAY="\033[0;39m" DEFAULT="\033[0m"
function echo() { command echo -e "$@"; }

function findtar_main() {
	local args=()
	function get_n() {
		local n=$(($1 + 1))
		shift
		eval echo \$${n}
	}
	function arg_n() { get_n ${1:-0} "${args[@]}"; }
	local dry_run='on'
	for OPT in "$@"; do
		case $OPT in
		'-r')
			local src_root=${1:-:}
			shift
			;;
		'-f')
			local dry_run=''
			;;
		'--')
			local find_options='ok'
			shift
			break
			;;
		*)
			local args=("${args[@]}" $1)
			;;
		esac
		shift
	done
	{ [[ -z $find_options ]] || [[ ${#args[@]} -lt 1 ]]; } && echo "$0 [options] [dst(dir, tar, tar.gz, tar.bz2)] -- [find commands]" && return 1
	[[ $src_root == ':' ]] && echo "$0 -r [src root dirpath]" && return 1

	local dst=$(arg_n 0)
	local last_ext=${dst##*.}
	local tar_opt=""
	local file_opt="-f"
	[[ $last_ext == "tar" ]] && local tar_opt="cv"
	[[ $last_ext == "gz" ]] && local tar_opt="cvz"
	[[ $last_ext == "bz2" ]] && local tar_opt="cvj"
	[[ -d $dst ]] && {
		local file_opt=""
		local tar_opt="c"
	}
	[[ $(dirname "${dst}/.") == '.' ]] && echo "Extract here is not meaning!" && return 1
	[[ -z $tar_opt ]] && echo "$dst has wrong ext! pick next from [tar, .tar.gz, tar.bz2]" && return 1

	if [[ -n $dry_run ]]; then
		echo "${PURPLE}#### dry-run ####${DEFAULT}" 1>&2
		find "$@"
		echo "${PURPLE}#### dry-run ####${DEFAULT}" 1>&2
		echo "" 1>&2
		echo "${YELLOW}#### FYI: how to send files to remote host (tar, tar.gz, tar.bz2) ####${DEFAULT}" 1>&2
		echo "find $@ -print0 | tar -c  -T - --null |& ssh \$HOST 'tar -C \$DIR -xv -'" 1>&2
		echo "find $@ -print0 | tar -cz -T - --null |& ssh \$HOST 'tar -C \$DIR -xv -'" 1>&2
		echo "find $@ -print0 | tar -cj -T - --null |& ssh \$HOST 'tar -C \$DIR -xv -'" 1>&2
		echo "${YELLOW}#### FYI: to run command not dry-run ####${DEFAULT}" 1>&2
		echo "${RED}add '-f' option before --" 1>&2
		return
	fi

	if [[ -n $file_opt ]]; then
		find "$@" -print0 | tar "$tar_opt" -T - --null -f "$dst"
		local exit_code=$?
	else
		find "$@" -print0 | tar "$tar_opt" -T - --null |& tar -C "$dst" -xv -
		local exit_code=$?
	fi

	echo "" 1>&2
	echo "${GREEN}If you want to extract here (for overwrite)${DEFAULT}" 1>&2
	echo "tar xvf $dst -C ." 1>&2
	return $exit_code
}

findtar_main "$@"
