import { useEffect, useRef } from 'react';
import { Terminal as XTerm } from 'xterm';
import { FitAddon } from 'xterm-addon-fit';
import 'xterm/css/xterm.css';
interface TerminalProps {
  id: string;
  title: string;
}
const Terminal = ({ id, title }: TerminalProps) => {
  const terminalRef = useRef<HTMLDivElement>(null);
  const xtermRef = useRef<XTerm | null>(null);
  useEffect(() => {
    if (!terminalRef.current) return;
    const term = new XTerm({
      theme: {
        background: '#000000',
        foreground: '#00ff00',
        cursor: '#00ff00',
        cursorAccent: '#000000',
        selection: 'rgba(0, 255, 0, 0.3)',
      },
      fontFamily: 'Courier New, monospace',
      fontSize: 14,
      cursorBlink: true,
      cursorStyle: 'block',
    });
    const fitAddon = new FitAddon();
    term.loadAddon(fitAddon);
    term.open(terminalRef.current);
    fitAddon.fit();
    term.write('SNS_PROV Terminal v1.0.0\r\n');
    term.write('> Жүйеге қосылу...\r\n');
    term.write('> Қауіпсіздік тексерісі...\r\n');
    term.write('> Қош келдіңіз!\r\n');
    term.write('\r\n$ ');
    term.onData(data => {
      term.write(data);
      if (data === '\r') {
        term.write('\n$ ');
      }
    });
    xtermRef.current = term;
    const handleResize = () => {
      fitAddon.fit();
    };
    window.addEventListener('resize', handleResize);
    return () => {
      term.dispose();
      window.removeEventListener('resize', handleResize);
    };
  }, []);
  return (
    <div className="terminal-window rounded-lg overflow-hidden backdrop-blur-sm">
      <div className="terminal-header flex items-center justify-between p-2">
        <span className="text-primary neon-text font-mono">{title}</span>
        <div className="flex gap-2">
          <div className="w-3 h-3 rounded-full bg-red-500"></div>
          <div className="w-3 h-3 rounded-full bg-yellow-500"></div>
          <div className="w-3 h-3 rounded-full bg-green-500"></div>
        </div>
      </div>
      <div ref={terminalRef} id={id} className="h-[300px]" />
    </div>
  );
};
export default Terminal;
