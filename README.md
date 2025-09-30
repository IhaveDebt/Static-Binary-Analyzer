/**
 * binary_analyzer.ts
 *
 * Very small parser for ELF/PE header detection and basic metadata extraction (by magic and sections).
 * This is not a full parser — it's a prototype to extract file type and a few bytes.
 *
 * Run:
 *  ts-node src/binary_analyzer.ts <path-to-binary>
 */
import fs from 'fs';
const path = process.argv[2];
if (!path) { console.log('Usage: ts-node src/binary_analyzer.ts <file>'); process.exit(1); }
const buf = fs.readFileSync(path);
function detect(buf:Buffer){
  if (buf.slice(0,4).toString('hex') === '7f454c46') return 'ELF';
  if (buf.slice(0,2).toString() === 'MZ') return 'PE';
  return 'UNKNOWN';
}
console.log('Type:', detect(buf));
console.log('Size bytes:', buf.length);
console.log('First 64 bytes (hex):', buf.slice(0,64).toString('hex').match(/.{1,32}/g));
