# Cookies

- **Platform:** picoCTF 2021
- **Difficulty:** Easy
- **Date completed:** 2026-10-21
- **Category:** Web Exploitation

## Summary
A simple site sets a cookie named `name` holding an integer, which the server uses server-side to look up and display a "favorite cookie" flavor. The flag is just another entry in that same lookup table, reachable by iterating through the possible index values.

## Steps
1. Inspected the site's cookies in the browser and found one named `name` with the value `-1`, on path `/` — unusual enough to investigate further, since it looked like it could be an array/list index used server-side.
2. Tried editing the cookie value directly in browser dev tools, but the page didn't visibly update — switched to `curl` instead for more reliable, repeatable control over the exact cookie sent:
   ```
   curl -s -b "name=2" http://chatelaine.cylabacademy.net:10454/check
   ```
3. Confirmed the index theory: different small integer values of `name` returned different "I love \<flavor\> cookies!" messages (e.g. `0` → snickerdoodle, `1` → chocolate chip, `2` → oatmeal raisin), while out-of-range or negative values (`-1`, `-2`, `100`) just redirected back to `/` instead of erroring out — meaning the lookup is bounds-checked rather than a raw negative-index trick.
4. Automated the search instead of testing values one at a time, looping through a range and printing only the non-redirect responses:
   ```bash
   for i in $(seq 0 200); do
     resp=$(curl -s -b "name=$i" http://chatelaine.cylabacademy.net:10454/check)
     if echo "$resp" | grep -q "Redirecting"; then
       continue
     fi
     echo "=== name=$i ==="
     echo "$resp" | grep -A2 "alert-success\|lead"
   done
   ```
5. The loop printed a different cookie flavor for each valid index from `0` through `17`, then at `name=18` the response broke pattern entirely and returned the flag directly instead of a flavor message.

## Tools used
- Browser dev tools — initial discovery of the suspicious `name` cookie (though editing cookies there proved unreliable for testing)
- `curl` — sent requests with arbitrary, exact cookie values without relying on the browser
- Bash `for` loop — automated iterating through candidate index values instead of testing manually one by one

## Lesson learned
A cookie holding a plain, unsigned integer that's clearly being used as a server-side array/lookup index is a strong IDOR signal — worth systematically walking through the valid range rather than guessing values individually. Automating the sweep (loop + grep for a distinguishing marker in the response) turns a tedious manual IDOR hunt into a one-command scan, and is much faster/more reliable than trying to edit cookies by hand in browser dev tools.

## Flag
```
academy{3v3ry1_l0v3s_c00k135_90a3a7cb}
```