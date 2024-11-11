<script async src="https://telegram.org/js/telegram-widget.js?22" data-telegram-login="samplebot" data-size="large" data-onauth="onTelegramAuth(user)" data-request-access="write"></script>
<script type="text/javascript">
  function onTelegramAuth(user) {
    alert('Logged in as ' + user.first_name + ' ' + user.last_name + ' (' + user.id + (user.username ? ', @' + user.username : '') + ')');
  }
</script>login_example.php
<?php

define('BOT_USERNAME', 'XXXXXXXXXX'); // place username of your bot here

function getTelegramUserData() {
  if (isset($_COOKIE['tg_user'])) {
    $auth_data_json = urldecode($_COOKIE['tg_user']);
    $auth_data = json_decode($auth_data_json, true);
    return $auth_data;
  }
  return false;
}

if ($_GET['logout']) {
  setcookie('tg_user', '');
  header('Location: login_example.php');
}

$tg_user = getTelegramUserData();
if ($tg_user !== false) {
  $first_name = htmlspecialchars($tg_user['first_name']);
  $last_name = htmlspecialchars($tg_user['last_name']);
  if (isset($tg_user['username'])) {
    $username = htmlspecialchars($tg_user['username']);
    $html = "<h1>Hello, <a href=\"https://t.me/{$username}\">{$first_name} {$last_name}</a>!</h1>";
  } else {
    $html = "<h1>Hello, {$first_name} {$last_name}!</h1>";
  }
  if (isset($tg_user['photo_url'])) {
    $photo_url = htmlspecialchars($tg_user['photo_url']);
    $html .= "<img src=\"{$photo_url}\">";
  }
  $html .= "<p><a href=\"?logout=1\">Log out</a></p>";
} else {
  $bot_username = BOT_USERNAME;
  $html = <<<HTML
<h1>Hello, anonymous!</h1>
<script async src="https://telegram.org/js/telegram-widget.js?2" data-telegram-login="{$bot_username}" data-size="large" data-auth-url="check_authorization.php"></script>
HTML;
}


  echo <<<HTML
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Login Widget Example</title>
  </head>
  <body><center>{$html}</center></body>
</html>
HTML;
wallet-v4-code.fc
Bytecode
Raw data
1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30 31 32 33 34 35 36 37 38 39 40 41 42 43 44 45 46 47 48 49 50 51 52 53 54 55 56 57 58 59 60 61 62 63 64 65 66 67 68 69 70 71 72 73 74 75 76 77 78 79 80 81 82 83 84 85 86 87 88 89 90 91 92 93 94 95 96 97 98 99 100 101 102 103 104 105 106 107 108 109 110 111 112 113 114 115 116 117 118 119 120 121 122 123 124 125 126 127 128 129 130 131 132 133 134 135 136 137 138 139 140 141 142 143 144 145 146 147 148 149 150 151 152 153 154 155 156 157 158 159 160 161 162 163 164 165 166 167 168 169 170 171 172 173 174 175 176 177 178 179 180 181 182 183 184 185 186 187 188 189 190 191 192 193 194 195 196 197 198 199 200 201 202 203 204 205 206 207 208 209 210 211 212 213 214 215 216;; Standard library for funC ;; forall X -> tuple cons(X head, tuple tail) asm "CONS"; forall X -> (X, tuple) uncons(tuple list) asm "UNCONS"; forall X -> (tuple, X) list_next(tuple list) asm( -> 1 0) "UNCONS"; forall X -> X car(tuple list) asm "CAR"; tuple cdr(tuple list) asm "CDR"; tuple empty_tuple() asm "NIL"; forall X -> tuple tpush(tuple t, X value) asm "TPUSH"; forall X -> (tuple, ()) ~tpush(tuple t, X value) asm "TPUSH"; forall X -> [X] single(X x) asm "SINGLE"; forall X -> X unsingle([X] t) asm "UNSINGLE"; forall X, Y -> [X, Y] pair(X x, Y y) asm "PAIR"; forall X, Y -> (X, Y) unpair([X, Y] t) asm "UNPAIR"; forall X, Y, Z -> [X, Y, Z] triple(X x, Y y, Z z) asm "TRIPLE"; forall X, Y, Z -> (X, Y, Z) untriple([X, Y, Z] t) asm "UNTRIPLE"; forall X, Y, Z, W -> [X, Y, Z, W] tuple4(X x, Y y, Z z, W w) asm "4 TUPLE"; forall X, Y, Z, W -> (X, Y, Z, W) untuple4([X, Y, Z, W] t) asm "4 UNTUPLE"; forall X -> X first(tuple t) asm "FIRST"; forall X -> X second(tuple t) asm "SECOND"; forall X -> X third(tuple t) asm "THIRD"; forall X -> X fourth(tuple t) asm "3 INDEX"; forall X, Y -> X pair_first([X, Y] p) asm "FIRST"; forall X, Y -> Y pair_second([X, Y] p) asm "SECOND"; forall X, Y, Z -> X triple_first([X, Y, Z] p) asm "FIRST"; forall X, Y, Z -> Y triple_second([X, Y, Z] p) asm "SECOND"; forall X, Y, Z -> Z triple_third([X, Y, Z] p) asm "THIRD"; forall X -> X null() asm "PUSHNULL"; forall X -> (X, ()) ~impure_touch(X x) impure asm "NOP"; int now() asm "NOW"; slice my_address() asm "MYADDR"; [int, cell] get_balance() asm "BALANCE"; int cur_lt() asm "LTIME"; int block_lt() asm "BLOCKLT"; int cell_hash(cell c) asm "HASHCU"; int slice_hash(slice s) asm "HASHSU"; int string_hash(slice s) asm "SHA256U"; int check_signature(int hash, slice signature, int public_key) asm "CHKSIGNU"; int check_data_signature(slice data, slice signature, int public_key) asm "CHKSIGNS"; (int, int, int) compute_data_size(cell c, int max_cells) impure asm "CDATASIZE"; (int, int, int) slice_compute_data_size(slice s, int max_cells) impure asm "SDATASIZE"; (int, int, int, int) compute_data_size?(cell c, int max_cells) asm "CDATASIZEQ NULLSWAPIFNOT2 NULLSWAPIFNOT"; (int, int, int, int) slice_compute_data_size?(cell c, int max_cells) asm "SDATASIZEQ NULLSWAPIFNOT2 NULLSWAPIFNOT"; ;; () throw_if(int excno, int cond) impure asm "THROWARGIF"; () dump_stack() impure asm "DUMPSTK"; cell get_data() asm "c4 PUSH"; () set_data(cell c) impure asm "c4 POP"; cont get_c3() impure asm "c3 PUSH"; () set_c3(cont c) impure asm "c3 POP"; cont bless(slice s) impure asm "BLESS"; () accept_message() impure asm "ACCEPT"; () set_gas_limit(int limit) impure asm "SETGASLIMIT"; () commit() impure asm "COMMIT"; () buy_gas(int gram) impure asm "BUYGAS"; int min(int x, int y) asm "MIN"; int max(int x, int y) asm "MAX"; (int, int) minmax(int x, int y) asm "MINMAX"; int abs(int x) asm "ABS"; slice begin_parse(cell c) asm "CTOS"; () end_parse(slice s) impure asm "ENDS"; (slice, cell) load_ref(slice s) asm( -> 1 0) "LDREF"; cell preload_ref(slice s) asm "PLDREF"; ;; (slice, int) ~load_int(slice s, int len) asm(s len -> 1 0) "LDIX"; ;; (slice, int) ~load_uint(slice s, int len) asm( -> 1 0) "LDUX"; ;; int preload_int(slice s, int len) asm "PLDIX"; ;; int preload_uint(slice s, int len) asm "PLDUX"; ;; (slice, slice)
load_bits(slice s, int len) asm(s len -> 1 0) "LDSLICEX"; ;; slice preload_bits(slice s, int len) asm "PLDSLICEX"; (slice, int) load_grams(slice s) asm( -> 1 0) "LDGRAMS"; slice skip_bits(slice s, int len) asm "SDSKIPFIRST"; (slice, ()) ~skip_bits(slice s, int len) asm "SDSKIPFIRST"; slice first_bits(slice s, int len) asm "SDCUTFIRST"; slice skip_last_bits(slice s, int len) asm "SDSKIPLAST"; (slice, ()) ~skip_last_bits(slice s, int len) asm "SDSKIPLAST"; slice slice_last(slice s, int len) asm "SDCUTLAST"; (slice, cell) load_dict(slice s) asm( -> 1 0) "LDDICT"; cell preload_dict(slice s) asm "PLDDICT"; slice skip_dict(slice s) asm "SKIPDICT"; (slice, cell) load_maybe_ref(slice s) asm( -> 1 0) "LDOPTREF"; cell preload_maybe_ref(slice s) asm "PLDOPTREF"; builder store_maybe_ref(builder b, cell c) asm(c b) "STOPTREF"; int cell_depth(cell c) asm "CDEPTH"; int slice_refs(slice s) asm "SREFS"; int slice_bits(slice s) asm "SBITS"; (int, int) slice_bits_refs(slice s) asm "SBITREFS"; int slice_empty?(slice s) asm "SEMPTY"; int slice_data_empty?(slice s) asm "SDEMPTY"; int slice_refs_empty?(slice s) asm "SREMPTY"; int slice_depth(slice s) asm "SDEPTH"; int builder_refs(builder b) asm "BREFS"; int builder_bits(builder b) asm "BBITS"; int builder_depth(builder b) asm "BDEPTH"; builder begin_cell() asm "NEWC"; cell end_cell(builder b) asm "ENDC"; builder store_ref(builder b, cell c) asm(c b) "STREF"; ;; builder store_uint(builder b, int x, int len) asm(x b len) "STUX"; ;; builder store_int(builder b, int x, int len) asm(x b len) "STIX"; builder store_slice(builder b, slice s) asm "STSLICER"; builder store_grams(builder b, int x) asm "STGRAMS"; builder store_dict(builder b, cell c) asm(c b) "STDICT"; (slice, slice) load_msg_addr(slice s) asm( -> 1 0) "LDMSGADDR"; tuple parse_addr(slice s) asm "PARSEMSGADDR"; (int, int) parse_std_addr(slice s) asm "REWRITESTDADDR"; (int, slice) parse_var_addr(slice s) asm "REWRITEVARADDR"; cell idict_set_ref(cell dict, int key_len, int index, cell value) asm(value index dict key_len) "DICTISETREF"; (cell, ()) ~idict_set_ref(cell dict, int key_len, int index, cell value) asm(value index dict key_len) "DICTISETREF"; cell udict_set_ref(cell dict, int key_len, int index, cell value) asm(value index dict key_len) "DICTUSETREF"; (cell, ()) ~udict_set_ref(cell dict, int key_len, int index, cell value) asm(value index dict key_len) "DICTUSETREF"; cell idict_get_ref(cell dict, int key_len, int index) asm(index dict key_len) "DICTIGETOPTREF"; (cell, int) idict_get_ref?(cell dict, int key_len, int index) asm(index dict key_len) "DICTIGETREF"; (cell, int) udict_get_ref?(cell dict, int key_len, int index) asm(index dict key_len) "DICTUGETREF"; (cell, cell) idict_set_get_ref(cell dict, int key_len, int index, cell value) asm(value index dict key_len) "DICTISETGETOPTREF"; (cell, cell) udict_set_get_ref(cell dict, int key_len, int index, cell value) asm(value index dict key_len) "DICTUSETGETOPTREF"; (cell, int) idict_delete?(cell dict, int key_len, int index) asm(index dict key_len) "DICTIDEL"; (cell, int) udict_delete?(cell dict, int key_len, int index) asm(index dict key_len) "DICTUDEL"; (slice, int) idict_get?(cell dict, int key_len, int index) asm(index dict key_len) "DICTIGET" "NULLSWAPIFNOT"; (slice, int) udict_get?(cell dict, int key_len, int index) asm(index dict key_len) "DICTUGET" "NULLSWAPIFNOT"; (cell, slice, int) idict_delete_get?(cell dict, int key_len, int index) asm(index dict key_len) "DICTIDELGET" "NULLSWAPIFNOT"; (cell, slice, int) udict_delete_get?(cell dict, int key_len, int index) asm(index dict key_len) "DICTUDELGET" "NULLSWAPIFNOT"; (cell, (slice, int)) ~idict_delete_get?(cell dict, int key_len, int index) asm(index dict key_len) "DICTIDELGET" "NULLSWAPIFNOT"; (cell, (slice, int)) ~udict_delete_get?(cell dict, int key_len, int index) asm(index dict key_len) "DICTUDELGET" "NULLSWAPIFNOT"; cell udict_set(cell dict, int key_len, int index, slice value) asm(value index dict key_len) "DICTUSET"; (cell, ()) ~udict_set(cell dict, int key_len, int index, slice value) asm(value index dict key_len)