======================
MinimumWindowSubstring
======================

java solution
-------------

.. code-block:: java
   :linenos:
   :emphasize-lines: 12,13,14

   class Solution {
        String result = "";

        // 1. validation
        if (s == null || s.length() == 0 || t == null || t.length() == 0) {
            return result;
        }

        // 2. initialization
        Map<Character, Integer> charCounterTarget = new HashMap<>();
        Map<Character, Integer> charCounterCurrent = new HashMap<>();
        int minLength = Integer.MAX_VALUE;
        int missedChars = 0;
        for (char c : t.toCharArray()) {
            charCounterTarget.put(c, charCounterTarget.getOrDefault(c, 0) + 1);
            missedChars++;
        }
        int end = 0;

        // 3. Sliding
        for (int start = 0; start < s.length(); start++) {
            while (end < s.length() && !valid(missedChars)) {
            // 4. update
            missedChars = update(s, end, charCounterTarget, charCounterCurrent, missedChars, true);
            end++;
            }

            if (valid(missedChars)) {
            // 5. result
            int currMinLength = end - start;
            if (minLength > currMinLength) {
                minLength = currMinLength;
                result = s.substring(start, end);
            }
            }

            // 6. cleanup
            missedChars = update(s, start, charCounterTarget, charCounterCurrent, missedChars, false);
        }

        return result;
        }

        private int update(String s, int idx, Map<Character, Integer> charCounterTarget,
            Map<Character, Integer> charCounterCurrent, int missedChars, boolean add) {
        char c = s.charAt(idx);
        int prevCount = charCounterCurrent.getOrDefault(c, 0);
        if (prevCount < charCounterTarget.getOrDefault(c, 0) && add) {
            missedChars--;
        } else if (prevCount <= charCounterTarget.getOrDefault(c, 0) && !add) {
            missedChars++;
        }
        int currCount = add ? prevCount + 1 : prevCount - 1;
        charCounterCurrent.put(c, currCount);
        return missedChars;
        }

        private boolean valid(int missedChars) {
        return missedChars == 0;
        }
   }

   Looks good