# CurveSwitch
private void prepareListData() {
        Intent intent = getIntent();
        String minY = String.valueOf(intent.getDoubleExtra("minY", 0));
        String maxY = String.valueOf(intent.getDoubleExtra("maxY", 0));
        String minX = intent.getStringExtra("minX");
        String maxX = intent.getStringExtra("maxX");
        String accuracy = intent.getStringExtra("accuracy");
        int j = intent.getIntExtra("j", 0);                            
        int h = intent.getIntExtra("h", 0);
        int evenness = intent.getIntExtra("evenness", 0); 
        double[] zeros = intent.getDoubleArrayExtra("zero");
        double[] sign = intent.getDoubleArrayExtra("sign");
        double[] derivatives = intent.getDoubleArrayExtra("deriv");
        double[] derivSigns = intent.getDoubleArrayExtra("derivSign");
        
        
        String strPos = "Positive values are within ", strNeg = "Negative values are within ";
        String strAsc = "The function increases in the intervals ", 
                strDes = "The function decreases in the intervals ";
        String strEvenness;
        
        if (evenness == 0)
            strEvenness = "Odd function";
        else if (evenness == 1)
            strEvenness = "Even function";
        else strEvenness = "Neither even nor odd function"; 
        
        for (int i = 0; i <= j; i++){
            if (sign[i] == 1) {
                if (strPos.length() > 28) strPos += " U ";
                if      (j == 0) strPos = "The function is always positive!";
                else if (i == 0) strPos += "[" + minX + ";" + zeros[i] + ")";
                else if (i == j) strPos += "(" + zeros[i-1] + ";" + maxX + "]";
                else             strPos += "(" + zeros[i-1] + ";" + zeros[i] + ")";
            }
            if (sign[i] == -1) {
                if (strNeg.length() > 28) strNeg += " U ";
                if      (j == 0) strNeg = "The function is always negative!";
                else if (i == 0) strNeg += "[" + minX + ";" + zeros[i] + ")";
                else if (i == j) strNeg += "(" + zeros[i-1] + ";" + maxX + "]";
                else             strNeg += "(" + zeros[i-1] + ";" + zeros[i] + ")";
            }
        }
        

        for (int i = 0; i <= h; i++){
            if (derivSigns[i] == 1) {
                if (strAsc.length() > 40) strAsc += " U ";
                if      (h == 0) strAsc = "The function always increases!";
                else if (i == 0) strAsc += "[" + minX + ";" + derivatives[i] + ")";
                else if (i == h) strAsc += "(" + derivatives[i-1] + ";" + maxX + "]";
                else             strAsc += "(" + derivatives[i-1] + ";" + derivatives[i] + ")";
            }
            if (derivSigns[i] == -1) {
                if      (h == 0) strDes = "The function always decreases!";
                else if (i == 0) strDes += "[" + minX + ";" + derivatives[i] + ")";
                else if (i == h) strDes += "(" + derivatives[i-1] + ";" + maxX + "]";
                else             strDes += "(" + derivatives[i-1] + ";" + derivatives[i] + ")";
            }
        }
        
        listDataHeader = new ArrayList<String>();
        listDataChild = new HashMap<String, List<String>>();
 
        listDataHeader.add("Domain and range of the function");
        listDataHeader.add("Zeros of the function");
        listDataHeader.add("f(0)");
        listDataHeader.add("Evenness of the function");
        listDataHeader.add("Monotonicity of the function");
        listDataHeader.add("Positive and negative values");
        listDataHeader.add("Local extrema");
 
        List<String> domainRange = new ArrayList<String>();
        domainRange.add("Having x within [" + minX + ";" + maxX + "] and accuracy " + accuracy + 
                  " y is within [" + minY + ";" + maxY + "]");
 
        List<String> functionZeros = new ArrayList<String>();
        for (int i = 0; i < j; i++)
            functionZeros.add("" + zeros[i]);
        
        List<String> zeroForY = new ArrayList<String>();
        zeroForY.add("When x equals 0 y is " + intent.getDoubleExtra("x=0", 0));

        List<String> evennessReport = new ArrayList<String>();
        evennessReport.add(strEvenness);
 
        List<String> monotonicity = new ArrayList<String>();
        if (strAsc.length() > 40 || strAsc.charAt(13) == 'a') 
            monotonicity.add(strAsc);
        if (strDes.length() > 40 || strDes.charAt(13) == 'a') 
            monotonicity.add(strDes);
        
        List<String> positivity = new ArrayList<String>();
        if (strPos.length() > 28 || strAsc.charAt(0) == 'T')
            positivity.add(strPos);
        if (strNeg.length() > 28 || strAsc.charAt(0) == 'T')
            positivity.add(strNeg);

        List<String> localExtrema = new ArrayList<String>();
        if (h > 0) {
            for (int i = 0; i < h; i++)
                localExtrema.add("" + derivatives[i]);
        }
        else localExtrema.add("There is no local extremum in this function.");
 
        listDataChild.put(listDataHeader.get(0), domainRange);
        listDataChild.put(listDataHeader.get(1), functionZeros);
        listDataChild.put(listDataHeader.get(2), zeroForY);
        listDataChild.put(listDataHeader.get(3), evennessReport);
        listDataChild.put(listDataHeader.get(4), monotonicity);
        listDataChild.put(listDataHeader.get(5), positivity);
        listDataChild.put(listDataHeader.get(6), localExtrema);
    }
}


