<%*
let start = moment().add(30-(moment().minute()%15),'minutes').startOf('minute');
tR += "- " + start.format('HH:mm') + " - \n";
// moment.js dates are mutable, so always only add 15 mins!
tR += "- " + start.add(25, 'minutes').format('HH:mm') + " - short break\n";
tR += "- " + start.add(5, 'minutes').format('HH:mm') + " - \n";
tR += "- " + start.add(25, 'minutes').format('HH:mm') + " - short break\n";
tR += "- " + start.add(5, 'minutes').format('HH:mm') + " - \n";
tR += "- " + start.add(25, 'minutes').format('HH:mm') + " - short break\n";
tR += "- " + start.add(5, 'minutes').format('HH:mm') + " - \n";
tR += "- " + start.add(25, 'minutes').format('HH:mm') + " - long break\n";
%>